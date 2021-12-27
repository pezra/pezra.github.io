---
id: 74
title: Java Daemon
date: 2005-03-25T12:00:59-07:00
author: Peter Williams
layout: post
guid: http://pezra.barelyenough.org/wordpress/?p=74
permalink: /blog/2005/03/java-daemon/
openid_comments:
  - 'a:1:{i:0;s:5:"85722";}'
aktt_notify_twitter:
  - 'yes'
categories:
  - Software Development
tags:
  - example
  - Java
---
Lately I have been writing a Java program that needs to run in the back ground (like a daemon). I found a couple of neat little tricks that can make this easier. These ideas probably only work in a Unix environment but they have been tested on Linux and Solaris.

So you have your program and you want to start it such that it will not be killed when you log out of the shell in which you start it. You could use nohup, but nohup redirects the standard out and error to files, which is annoying because you are writing all output to a log file anyway. You could do <kbd>java -cp your_class_path com.domain.main_class <&- 1>/dev/null 2>&1 &</kbd> which runs the program in the background, closes standard in, and redirects standard out and error to the bit bucket. By closing standard in to the process the shell will not kill the program when it exits and it is running in the background so it will not interfere with other actions we might want to perform with this shell.

However, it would be nice if you could print errors that occur during startup to the prompt &#8212; for example if the config file were missing. This is nice because it is generally appropriate to sanity check the configuration so that if the program starts there is a significant chance it will actually work correctly. So let&#8217;s not redirect standard out and error. That leaves you with  
<kbd>java -cp your_class_path com.domain.main_class <&- &</kbd>, which works but a shell will not exit while there are still programs attached to its standard out and error &#8212; as this one is.

The solution is to have the Java program detach from standard out and error once its startup is complete. So we will create a method called daemonize() that we will call just before entering the infinite loop that is the main program logic.

<pre class='code'>static public void daemonize()
{
   System.out.close();
   System.err.close();
}
</pre>

So now we have a main method which looks like

<pre class='code'>static public void main(String[] args)
{
   try
   {
       // do sanity checks and startup actions
       daemonize();
   }
   catch (Throwable e)
   {
       System.err.println("Startup failed.");
       e.printStackTrace();
   }

   // do infinite loop
}
</pre>

Now when we start our program we get to see if it started correctly and if it does start it will not be killed when the shell exits nor will it prevent the shell from exiting.

Now that the program is completely detached from the shell the only way to stop it is by killing the process. However, to do that you need to know the pid. Java has no way for a program to figure its pid directly &#8212; it is too system dependent. So we will create a shell script to launch our daemon and record its pid for future use. 

<pre class="code">#!/bin/sh
java -cp your_class_path com.domain.main_class &lt;&amp;- &
pid=$!
echo ${pid} &gt; mydaemon.pid
</pre>

This script launches the program and then writes the pid to the file &#8216;mydaemon.pid&#8217;. Now when you want to kill the program you can do  
<kbd><br /> kill `cat mydaemon.pid`<br /> </kbd>  
There are a couple of problems with this, one is that the pid file gets written even if the daemon failed to start successfully, another is that if the daemon crashes the pid file will still exist &#8212; which might lead you to believe it is still working.

We can solve the pid file surviving daemon crash problem by passing it into the Java program like the following  
<kbd><br /> java -Ddaemon.pidfile=mydaemon.pid -cp your_class_path com.domain.main_class <&- &<br /> </kbd>  
And the updated the daemonize() method to the following

.

<pre class='code'>static public void daemonize()
{
   getPidFile().deleteOnExit();
   System.out.close();
   System.err.close();
}
</pre>

where getPidFile() is a method which returns a File object file specified by the system property &#8220;daemon.pidfile&#8221;. That way the pid file will be deleted when the VM exits.

For the overly eager creation of the pid file you could add a delay to the shell script and then check to make sure the process is still running before writing the pid file. But how long should the delay be? A better way is to take advantage of the fact the a shell will not exit while a program is attached to its standard out or error &#8212; even if the process is running in the background. We know that our program will not detach from those until the startup process is complete. The following shell script achieves this

<pre class="code">#!/bin/sh

launch_daemon()
{
  /bin/sh &lt;&lt;EOF
     java -Ddaemon.pidfile=mydaemon.pid -cp your_class_path com.domain.main_class &lt;&amp;- &
     pid=\$!
     echo \${pid}
EOF
}

daemon_pid=`launch_daemon`
if ps -p "${daemon_pid}" &gt;/dev/null 2&gt;&1
then
  # daemon is running.
  echo ${daemon_pid} &gt; mydaemon.pid
else
  echo "Daemon did not start."
fi
</pre>

This script starts a sub-shell and launches the daemon (in the launch\_daemon() function). The sub-shell will only return once the java program has detached from the console &#8212; for our program that means it has completed its startup or died. After the launch\_daemon() function returns we check to see if the pid it started is still running. If so it means that the daemon started correctly and the we write the daemon&#8217;s pid to the pid file. Remember that whenever the daemon&#8217;s VM shuts down the pid file will be deleted so you can treat the existence of the pid file as an indication that the process is running.

Now it occurs to you that if a problem occurs during startup you really would like to log it to both the log file and console. Since you are using log4j this is pretty straight forward. Just updated you main method like the following

<pre class='code'>static public void main(String[] args)
{
   Appender startupAppender = new ConsoleAppender(new SimpleLayout(), "System.err");
   try
   {
       logger.addAppender(startupAppender);
       // do sanity checks and startup actions
       daemonize();
   }
   catch (Throwable e)
   {
       logger.fatal("Startup failed.",e);
   }
   finally
   {
      logger.removeAppender(startupAppender);
   }

   // do infinite loop
}
</pre>

where &#8220;logger&#8221; is a static member variable that contains a Logger object. The nice thing about this is you can log message anywhere in you startup code and know that someone will see them, even if it occurs before you have configured the logging based on the application configuration. If the normal application logging is configured, the messages will go both to the console and to the log file for future debugging.

So all that works pretty well. There is another problem though. There is not a clean way to shut this daemon down. We need a graceful way to handle shutdown. So we add the following code to our main class

<pre class='code'>static protected boolean shutdownRequested = false;

static public void shutdown()
{
   shutdownRequested = true;
}

static public isShutdownRequested()
{
   return shutdownRequested;
}
</pre>

Then we update our application so that occasionally it checks &#8216;isShutdownRequested()&#8217; and if it is we leave the main loop. Now our main method looks like

<pre class='code'>static public void main(String[] args)
{
   Appender startupAppender = new ConsoleAppender(new SimpleLayout());
   try
   {
       logger.addAppender(startupAppender);
       // do sanity checks and startup actions
       daemonize();
   }
   catch (Throwable e)
   {
       logger.fatal("Startup failed.",e);
   }
   finally
   {
      logger.removeAppender(startupAppender);
   }

   while(!isShutdownRequested())
   {
      // wait for stimuli
      // process stimulus
   }
}
</pre>

This looks pretty good but you still only shutdown from inside the program. The solution is a VM shutdown hook. The is a bit of code that the VM runs when it is shutdown. We create the following method

<pre class='code'>static protected void addDaemonShutdownHook()
{
   Runtime.getRuntime().addShutdownHook( new Thread() { public void run() { MainClass.shutdown(); }});
}</pre>

and update the shutdown method as follows

<pre class='code'>static public void shutdown()
{
   shutdownRequested = true;

   try
   {
       getMainDaemonThread().join();
   }
   catch(InterruptedException e)
   {
       logger.error("Interrupted which waiting on main daemon thread to complete.");
   }
}
</pre>

Note that we now wait for the main daemon thread to die. This is because the VM waits for the VM shutdown hooks to complete for exiting but it does _not_ wait for other threads to complete. This join allows the main daemon threads to complete in a controlled way rather than being killed by the VM. Then we update the main method to call this new addDaemonShutdownHook() method

<pre class='code'>static public void main(String[] args)
{
   Appender startupAppender = new ConsoleAppender(new SimpleLayout());
   try
   {
       logger.addAppender(startupAppender);
       // do sanity checks and startup actions
       daemonize();
       addDaemonShutdownHook();
   }
   catch (Throwable e)
   {
       logger.fatal("Startup failed.",e);
   }
   finally
   {
      logger.removeAppender(startupAppender);
   }

   while(!isShutdownRequested())
   {
      // wait for stimuli
      // process stimulus
   }

   // do shutdown actions
}
</pre>

Now you can kill the process using <kbd>kill `cat mydaemon.pid`</kbd> but shutdown will be orderly and controlled.

So there you have it. A fairly safe and full featured way of create a Unix daemon with Java. Of course, if you do not need the extra control and do not mind have native binaries it might be easier to use [Jakarta Daemon](http://jakarta.apache.org/commons/daemon/).

<span style='font-style: italic;'>{Update: corrected a variable name in one of the shell scripts and added a needed closing brace to one of the bits of Java code}</span>