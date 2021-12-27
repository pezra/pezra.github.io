---
id: 1542
title: How i judge software engineers
date: 2018-03-12T22:32:32-06:00
author: Peter Williams
layout: post
guid: http://barelyenough.org/?p=1542
permalink: /blog/2018/03/how-i-judge-software-engineers/
categories:
  - Software Development
---
My kid&#8217;s teachers routinely provide rubrics for assignments. At first blush, rubrics are tools to make grading an assessment easier. They are effective in that role. They turn out to be at least as effective at communicating expectations. Readers of a rubric can quickly and easily determine what is important.

Recently i developed a rubric to help me judge the performance of engineers (it was review season yet again). The engineers on my team have appreciated the transparency and clarity this tool provides. Hopefully, it will be helpful to others.

The Rubric is divided into two sections. One about results and the other about behavior. Both of these are important. Good, pro-social behavior is just as important in engineers as strong results.

### Results

<table style="font-size: 80%;">
  <tr>
    <th colspan="1" rowspan="1">
    </th>
    
    <th colspan="1" rowspan="1">
      3
    </th>
    
    <th colspan="1" rowspan="1">
      2
    </th>
    
    <th colspan="1" rowspan="1">
      1
    </th>
    
    <th colspan="1" rowspan="1">
    </th>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Continuous improvement
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          always leaves code substantially better than they found it
        </li>
        <li>
          manages scope of refactors to match time available
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          often improves code as part of normal work
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          sometimes improves code as part of normal work
        </li>
        <li>
          often has to abandon refactors due to time constraints
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          rarely improves existing code
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Communication
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          communicates intentions early and clearly
        </li>
        <li>
          often provides material support to teammates (developers, QA, on-call person, etc)
        </li>
        <li>
          regularly creates & maintains runbook (particularly when on call)
        </li>
        <li>
          keeps stakeholder informed (particularly when on call)
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          communicates intentions after it is hard to change course
        </li>
        <li>
          regularly supports teammates
        </li>
        <li>
          occasionally maintains runbook
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          rarely communicates intentions
        </li>
        <li>
          sometimes supports teammates
        </li>
        <li>
          doesn&#8217;t maintain runbook
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          never communicates intentions
        </li>
        <li>
          never supports teammates
        </li>
        <li>
          never creates or improves runbook entries
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Production support
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          prioritizes concurrent incidents correctly
        </li>
        <li>
          use critical thinking and problem-solving skills to resolve issues quickly
        </li>
        <li>
          handles lower priority issues (eg, jenkins nodes down) when there are no higher priority incidents
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          prioritizes concurrent incidents correctly
        </li>
        <li>
          use critical thinking and problem solving skills to resolve issues
        </li>
        <li>
          handles lower priority issues (eg, jenkins nodes down) when there are no higher priority incidents
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          prioritizes concurrent incidents correctly
        </li>
        <li>
          ignores lower priority issues (eg, jenkins nodes down) even when there are no high priority incidents (works stories while on call)
        </li>
        <li>
          relies too heavily on others (rather than using critical thinking and problem-solving skills)
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          bad attitude
        </li>
        <li>
          incorrectly prioritizes concurrent incidents
        </li>
        <li>
          always ignores lower priority issues (jenkins nodes down)
        </li>
        <li>
          doesn&#8217;t communicate incident status to stakeholders
        </li>
        <li>
          relies on others to resolve issues (throws it over the wall)
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Code Quality
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          functional
        </li>
        <li>
          well factored
        </li>
        <li>
          documentation on classes/modules and public method/functions
        </li>
        <li>
          PRs require trivial changes
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          functional
        </li>
        <li>
          well factored
        </li>
        <li>
          poorly documented
        </li>
        <li>
          PRs require minor changes
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          functional
        </li>
        <li>
          poorly factored
        </li>
        <li>
          undocumented
        </li>
        <li>
          PRs require some rework
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          buggy
        </li>
        <li>
          poorly factored
        </li>
        <li>
          undocumented
        </li>
        <li>
          PRs require substantial rework
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Productivity
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          usually delivers moreÂ  stories per sprint than the average engineer
        </li>
        <li>
          usually delivers more points per sprint than the average engineer
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          occasionally delivers more stories/points per sprint than the average engineer
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          usually delivers fewer stories/points per sprint than the average engineer
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          delivers substantially fewer stories/points per sprint than the average engineer
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Testing
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          public contracts well tested
        </li>
        <li>
          key scenarios have acceptance tests
        </li>
        <li>
          tests are independent of current implementation
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          public contract partially tested
        </li>
        <li>
          key scenarios have acceptance tests
        </li>
        <li>
          tests are independent or current implementation
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          public contract partially tested
        </li>
        <li>
          tests dependent on current implementation
        </li>
        <li>
          acceptance tests check too many edge cases
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          no unit or functional tests
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Feedback
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          often reviews PRs
        </li>
        <li>
          feedback is substantive and useful
        </li>
        <li>
          reviews show understanding of PRs intent and the code that interacts with it
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          regularly reviews PRs
        </li>
        <li>
          advice would materially improve PRs
        </li>
        <li>
          reviews show understanding of PRs intent
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          sometimes reviews PRs
        </li>
        <li>
          reviews are superficial
        </li>
        <li>
          reviews are hard to understand
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          never reviews PRs
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Product & domain knowledge
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          understands the domain
        </li>
        <li>
          understands most of the supported features of the product
        </li>
        <li>
          understands some of the historical features of the product
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          understands the domain
        </li>
        <li>
          understands many of the supported features of the product
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          some knowledge of the domains
        </li>
        <li>
          limited knowledge of product features
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          no knowledge of utility and grid edge domain
        </li>
        <li>
          no knowledge of product
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Personal goals
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          goals are SMART
        </li>
        <li>
          goals drive achievement of team and corporate goals in material ways
        </li>
        <li>
          achieves goals on time
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          goals are SMART
        </li>
        <li>
          goals weakly support team and corporate goals
        </li>
        <li>
          achieves goals
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          goals have no relation to team and corporate goals
        </li>
        <li>
          achieves goals
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          goals are vague or unattainable
        </li>
        <li>
          goals work against team and corporate goals
        </li>
        <li>
          doesn&#8217;t achieve goals
        </li>
      </ul>
    </td>
  </tr>
</table>

### Behavior

<table style="font-size: 80%;">
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Attitude
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          polite and engaging even when under stress (eg, when on call)
        </li>
        <li>
          accepts setbacks and moves forward
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          normally polite but brusque when under stress
        </li>
        <li>
          accepts setbacks and moves forward
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          normally polite but rude when under stress
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          rude
        </li>
        <li>
          dismissive
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Courage
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          courageous in all aspects of work every day
        </li>
        <li>
          strives for greatness even when difficult
        </li>
        <li>
          occasionally fails spectacularly
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          often courageous in most aspects of work
        </li>
        <li>
          occasionally fails
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          sometimes courageous in some aspects of work
        </li>
        <li>
          rarely fails
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          often timid
        </li>
        <li>
          usually takes least risky (and rewarding) approach
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Motivation
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          highly motivated to succeed
        </li>
        <li>
          accepts challenges and new responsibilities
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          motivated to succeed
        </li>
        <li>
          grudgingly accepts new challenges and responsibilities
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          resists new challenges and responsibilities
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          lacks motivation
        </li>
        <li>
          rejects all new challenges and responsibilities
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Strategic thinking
    </th>
    
    <td colspan="1" rowspan="1">
      plans for 6 month &#8211; 2 year horizon
    </td>
    
    <td colspan="1" rowspan="1">
      plans for 3 &#8211; 6 month horizon
    </td>
    
    <td colspan="1" rowspan="1">
      plans for 1 &#8211; 3 month horizon
    </td>
    
    <td colspan="1" rowspan="1">
      no planning
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Trustworthiness
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          earns trust of others
        </li>
        <li>
          reliably meets commitments
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          honest
        </li>
        <li>
          occasionally fails to meet commitments
        </li>
        <li>
          sometimes fails to gain the trust of others
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          fails to meet commitments
        </li>
        <li>
          occasionally misconstrues the facts
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          often misconstrues the facts
        </li>
        <li>
          widely distrusted
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Learning
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          seeks out learning opportunities
        </li>
        <li>
          applies lessons learn to enhance success
        </li>
        <li>
          elicits relevant experiences from others
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          interested in learning
        </li>
        <li>
          applies lessons learn to enhance success
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          learns when pushed
        </li>
        <li>
          resists change
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          uninterested in learning
        </li>
        <li>
          resists change
        </li>
      </ul>
    </td>
  </tr>
  
  <tr>
    <th style="white-space: unset;" colspan="1" rowspan="1">
      Pairing*
    </th>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          improves productivity and morale of pairs
        </li>
        <li>
          pairs most of the time
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          pairs effectively
        </li>
        <li>
          pairs most of the time
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          pairs ineffectively
        </li>
        <li>
          pairs some of the time
        </li>
      </ul>
    </td>
    
    <td colspan="1" rowspan="1">
      <ul>
        <li>
          reduces productivity and morale of pairs
        </li>
        <li>
          rarely pairs
        </li>
      </ul>
    </td>
  </tr>
</table>

* Optional. If your team pairs at a matter of course this is **very** important. If your team works as individuals then ignore this row.