---
layout: post
title:  "Grouping by Regex in Oracle PL/SQL"
date:   2020-03-29
categories: [coding]
tags: [sql, regex]
---

With Oracle PL/SQL you can use built in regular expression functions to match query results using SELECT statements. More on that documentation can be found [here in the oracle reference documentation](https://docs.oracle.com/cd/B28359_01/server.111/b28286/functions138.htm#SQLRF06303).

This entry will use the REGEX_SUBSTR() function to find strings that match the given pattern. To group by substring results, a nested select statement is used to refer to the named column. As a reference we will use Oracle's built in [HR schema Employee table](https://docs.oracle.com/database/121/COMSC/installation.htm#COMSC00002).

As an example we can determine the maximum salary for each department using only the JOB_ID field:

1\. Explore the data

  {% highlight sql %}

  SELECT FIRST_NAME, LAST_NAME, JOB_ID, SALARY FROM Employees;
  {% endhighlight %}

  <br>
  <center>
  <table>
    <thead><tr>	<th>FIRST_NAME</th>
    	<th>LAST_NAME</th>
    	<th>JOB_ID</th>
    	<th>SALARY</th>
    </tr></thead>
    <tbody id="data">

    	<tr>
    <td align="center">Steven</td>
    <td align="center">King</td>
    <td align="center">AD_PRES</td>
    <td align="right">24000</td>
    	</tr>
    	<tr>
    <td align="center">Neena</td>
    <td align="center">Kochhar</td>
    <td align="center">AD_VP</td>
    <td align="right">17000</td>
    	</tr>
    	<tr>
    <td align="center">Lex</td>
    <td align="center">De Haan</td>
    <td align="center">AD_VP</td>
    <td align="right">17000</td>
    	</tr>
    	<tr>
    <td align="center">Alexander</td>
    <td align="center">Hunold</td>
    <td align="center">IT_PROG</td>
    <td align="right">9000</td>
    	</tr>
    	<tr>
    <td align="center">Bruce</td>
    <td align="center">Ernst</td>
    <td align="center">IT_PROG</td>
    <td align="right">6000</td>
    	</tr>
    	<tr>
    <td align="center">David</td>
    <td align="center">Austin</td>
    <td align="center">IT_PROG</td>
    <td align="right">4800</td>
    	</tr>
    	<tr>
    <td align="center">Valli</td>
    <td align="center">Pataballa</td>
    <td align="center">IT_PROG</td>
    <td align="right">4800</td>
    	</tr>
      <tr>
        <td align="center">...</td>
        <td align="center">...</td>
        <td align="center">...</td>
        <td align="center">...</td>
      </tr>
      </tbody>
  </table>
  </center>

<br>

2\. Use a nested select statement to extract the department from JOB_ID and calculate the maximum salary by department using a GROUP BY clause. The pattern '^\w{2}' means to select the first two alphabet characters at the start of the string.


  {% highlight sql %}

  SELECT DEPARTMENT, MAX(SALARY) FROM (

      SELECT
          REGEXP_SUBSTR(JOB_ID,'^\w{2}') "DEPARTMENT",
          SALARY
      FROM Employees)

  GROUP BY DEPARTMENT;
  {% endhighlight %}

  <br>
  <center>
  <table>
    <thead><tr>	<th>DEPARTMENT</th>
    	<th>MAX(SALARY)</th>
    </tr></thead>
    <tbody id="data">

    	<tr>
    <td align="center">FI</td>
    <td align="right">12008</td>
    	</tr>
    	<tr>
    <td align="center">ST</td>
    <td align="right">8200</td>
    	</tr>
    	<tr>
    <td align="center">HR</td>
    <td align="right">6500</td>
    	</tr>
    	<tr>
    <td align="center">AD</td>
    <td align="right">24000</td>
    	</tr>
    	<tr>
    <td align="center">SA</td>
    <td align="right">14000</td>
    	</tr>
    	<tr>
    <td align="center">MK</td>
    <td align="right">13000</td>
    	</tr>
      	<tr>
      <td align="center">PR</td>
      <td align="right">10000</td>
      	</tr>
      	<tr>
      <td align="center">AC</td>
      <td align="right">12008</td>
      	</tr>
      	<tr>
      <td align="center">IT</td>
      <td align="right">9000</td>
      	</tr>
      	<tr>
      <td align="center">PU</td>
      <td align="right">11000</td>
      	</tr>
      	<tr>
      <td align="center">SH</td>
      <td align="right">4200</td>
      	</tr>
      </tbody>
  </table>
  </center>


A useful tool for checking regular expressions is [regex101](regex101.com).
