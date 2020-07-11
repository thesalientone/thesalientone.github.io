---
layout: post
title:  "Utility of List Aggregation Function"
date:   2020-06-28
categories: [coding]
tags: [sql]
---

The list aggregation function in SQL is a very handy function for consolidating data from multiple rows in data based on a grouping variable. Be sure to try out this function using [Oracle's live SQL editor](https://livesql.oracle.com/) SQL Worksheet tool.

With Oracle PL/SQL you there is a built-in function for list aggregation with details [found in the reference documentation](https://docs.oracle.com/cd/E11882_01/server.112/e41084/functions089.htm#SQLRF30030).

This entry will show an example generating a CSV list of each of the salaries based on Job IDs. For this example we will use Oracle's built in [HR schema Employee table](https://docs.oracle.com/database/121/COMSC/installation.htm#COMSC00002).

**Syntax**

<span style="color:blue">LISTAGG</span>(<span style="color:purple">COLUMN_NAME</span>, '<span style="color:chocolate">.,delimiter</span>') WITHIN GROUP(ORDER BY <span style="color:purple">GROUPING_VAR1</span>, <span style="color:purple">GROUPUING_VAR2</span>,<span style="color:purple">..</span>)

Followed eventually by a grouping clause:

GROUP BY <span style="color:purple">GROUPING_VAR1</span>, <span style="color:purple">GROUPUING_VAR2</span>,<span style="color:purple">..</span>

**Steps**

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

2\. Run a query to aggregate the salaries grouped by the job ID. The delimiter used can vary but commas or semi-colons are very readable. When joining text spaces can also be used. .


  {% highlight sql %}

  SELECT
     JOB_ID,
     LISTAGG(SALARY, ',') WITHIN GROUP (ORDER BY JOB_ID) "Salaries"
  FROM HR.EMPLOYEES
  GROUP BY JOB_ID;
  {% endhighlight %}

  <br>
  <center>
  <div style="overflow:scroll">
  <table summary="Results from LISTAGG Query" class="wrap-cells" style="">
<tbody><tr><th id="JOB_ID_224851685562367756115273885240187851200">JOB_ID</th><th id="Salaries_224851685562367756115273885240187851200">Salaries</th></tr><tr><td headers="JOB_ID">AC_ACCOUNT</td><td headers="Salaries">8300</td></tr><tr><td headers="JOB_ID">AC_MGR</td><td headers="Salaries">12008</td></tr><tr><td headers="JOB_ID">AD_ASST</td><td headers="Salaries">4400</td></tr><tr><td headers="JOB_ID">AD_PRES</td><td headers="Salaries">24000</td></tr><tr><td headers="JOB_ID">AD_VP</td><td headers="Salaries">17000,17000</td></tr><tr><td headers="JOB_ID">FI_ACCOUNT</td><td headers="Salaries">6900,7700,7800,8200,9000</td></tr><tr><td headers="JOB_ID">FI_MGR</td><td headers="Salaries">12008</td></tr><tr><td headers="JOB_ID">HR_REP</td><td headers="Salaries">6500</td></tr><tr><td headers="JOB_ID">IT_PROG</td><td headers="Salaries">4200,4800,4800,6000,9000</td></tr><tr><td headers="JOB_ID">MK_MAN</td><td headers="Salaries">13000</td></tr><tr><td headers="JOB_ID">MK_REP</td><td headers="Salaries">6000</td></tr><tr><td headers="JOB_ID">PR_REP</td><td headers="Salaries">10000</td></tr><tr><td headers="JOB_ID">PU_CLERK</td><td headers="Salaries">2500,2600,2800,2900,3100</td></tr><tr><td headers="JOB_ID">PU_MAN</td><td headers="Salaries">11000</td></tr><tr><td headers="JOB_ID">SA_MAN</td><td headers="Salaries">10500,11000,12000,13500,14000</td></tr><tr><td headers="JOB_ID">SA_REP</td><td headers="Salaries">10000,10000,10000,10500,11000,11500,6100,6200,6200,6400,6800,7000,7000,7000,7200,7300,7400,7500,7500,8000,8000,8400,8600,8800,9000,9000,9500,9500,9500,9600</td></tr><tr><td headers="JOB_ID">SH_CLERK</td><td headers="Salaries">2500,2500,2600,2600,2800,2800,2900,3000,3000,3100,3100,3200,3200,3400,3600,3800,3900,4000,4100,4200</td></tr><tr><td headers="JOB_ID">ST_CLERK</td><td headers="Salaries">2100,2200,2200,2400,2400,2500,2500,2500,2600,2700,2700,2800,2900,3100,3200,3200,3300,3300,3500,3600</td></tr><tr><td headers="JOB_ID">ST_MAN</td><td headers="Salaries">5800,6500,7900,8000,8200</td></tr>
</tbody></table>
</div>
  </center>



3\. Sometimes it is also useful to only select only two or three of the items being consolidated. To do this use the [PARTITION](https://docs.oracle.com/cd/B19306_01/server.102/b14200/functions001.htm#i81407)  function combined with a [WHERE](https://docs.oracle.com/cd/B19306_01/server.102/b14200/statements_10002.htm#i2134734) filter. The below query limits the selection to only 3 salaries per Job ID.


{% highlight sql %}
SELECT * FROM (
  SELECT

    ROW_NUMBER() OVER(PARTITION BY JOB_ID ORDER BY JOB_ID) "NUM",
    E.*,
  FROM HR.EMPLOYEES E
  )
WHERE NUM <= 3 ;
{% endhighlight %}
<br>
<div style="overflow:scroll">
<table summary="Results from query 2" class="u-Report">
<tbody><tr><th id="NUM_226206005508868038463822230207580502176">NUM</th><th id="EMPLOYEE_ID_226206005508868038463822230207580502176">EMPLOYEE_ID</th><th id="FIRST_NAME_226206005508868038463822230207580502176">FIRST_NAME</th><th id="LAST_NAME_226206005508868038463822230207580502176">LAST_NAME</th><th id="EMAIL_226206005508868038463822230207580502176">EMAIL</th><th id="PHONE_NUMBER_226206005508868038463822230207580502176">PHONE_NUMBER</th><th id="HIRE_DATE_226206005508868038463822230207580502176">HIRE_DATE</th><th id="JOB_ID_226206005508868038463822230207580502176">JOB_ID</th><th id="SALARY_226206005508868038463822230207580502176">SALARY</th><th id="COMMISSION_PCT_226206005508868038463822230207580502176">COMMISSION_PCT</th><th id="MANAGER_ID_226206005508868038463822230207580502176">MANAGER_ID</th><th id="DEPARTMENT_ID_226206005508868038463822230207580502176">DEPARTMENT_ID</th></tr><tr><td headers="NUM">1</td><td headers="EMPLOYEE_ID">206</td><td headers="FIRST_NAME">William</td><td headers="LAST_NAME">Gietz</td><td headers="EMAIL">WGIETZ</td><td headers="PHONE_NUMBER">515.123.8181</td><td headers="HIRE_DATE">07-JUN-02</td><td headers="JOB_ID">AC_ACCOUNT</td><td headers="SALARY">8300</td><td headers="COMMISSION_PCT"> - </td><td headers="MANAGER_ID">205</td><td headers="DEPARTMENT_ID">110</td></tr><tr><td headers="NUM">1</td><td headers="EMPLOYEE_ID">205</td><td headers="FIRST_NAME">Shelley</td><td headers="LAST_NAME">Higgins</td><td headers="EMAIL">SHIGGINS</td><td headers="PHONE_NUMBER">515.123.8080</td><td headers="HIRE_DATE">07-JUN-02</td><td headers="JOB_ID">AC_MGR</td><td headers="SALARY">12008</td><td headers="COMMISSION_PCT"> - </td><td headers="MANAGER_ID">101</td><td headers="DEPARTMENT_ID">110</td></tr><tr><td headers="NUM">1</td><td headers="EMPLOYEE_ID">200</td><td headers="FIRST_NAME">Jennifer</td><td headers="LAST_NAME">Whalen</td><td headers="EMAIL">JWHALEN</td><td headers="PHONE_NUMBER">515.123.4444</td><td headers="HIRE_DATE">17-SEP-03</td><td headers="JOB_ID">AD_ASST</td><td headers="SALARY">4400</td><td headers="COMMISSION_PCT"> - </td><td headers="MANAGER_ID">101</td><td headers="DEPARTMENT_ID">10</td></tr><tr><td headers="NUM">1</td><td headers="EMPLOYEE_ID">100</td><td headers="FIRST_NAME">Steven</td><td headers="LAST_NAME">King</td><td headers="EMAIL">SKING</td><td headers="PHONE_NUMBER">515.123.4567</td><td headers="HIRE_DATE">17-JUN-03</td><td headers="JOB_ID">AD_PRES</td><td headers="SALARY">24000</td><td headers="COMMISSION_PCT"> - </td><td headers="MANAGER_ID"> - </td><td headers="DEPARTMENT_ID">90</td></tr><tr><td headers="NUM">1</td><td headers="EMPLOYEE_ID">101</td><td headers="FIRST_NAME">Neena</td><td headers="LAST_NAME">Kochhar</td><td headers="EMAIL">NKOCHHAR</td><td headers="PHONE_NUMBER">515.123.4568</td><td headers="HIRE_DATE">21-SEP-05</td><td headers="JOB_ID">AD_VP</td><td headers="SALARY">17000</td><td headers="COMMISSION_PCT"> - </td><td headers="MANAGER_ID">100</td><td headers="DEPARTMENT_ID">90</td></tr><tr><td headers="NUM">2</td><td headers="EMPLOYEE_ID">102</td><td headers="FIRST_NAME">Lex</td><td headers="LAST_NAME">De Haan</td><td headers="EMAIL">LDEHAAN</td><td headers="PHONE_NUMBER">515.123.4569</td><td headers="HIRE_DATE">13-JAN-01</td><td headers="JOB_ID">AD_VP</td><td headers="SALARY">17000</td><td headers="COMMISSION_PCT"> - </td><td headers="MANAGER_ID">100</td><td headers="DEPARTMENT_ID">90</td></tr><tr><td headers="NUM">1</td><td headers="EMPLOYEE_ID">109</td><td headers="FIRST_NAME">Daniel</td><td headers="LAST_NAME">Faviet</td><td headers="EMAIL">DFAVIET</td><td headers="PHONE_NUMBER">515.124.4169</td><td headers="HIRE_DATE">16-AUG-02</td><td headers="JOB_ID">FI_ACCOUNT</td><td headers="SALARY">9000</td><td headers="COMMISSION_PCT"> - </td><td headers="MANAGER_ID">108</td><td headers="DEPARTMENT_ID">100</td></tr><tr><td headers="NUM">2</td><td headers="EMPLOYEE_ID">110</td><td headers="FIRST_NAME">John</td><td headers="LAST_NAME">Chen</td><td headers="EMAIL">JCHEN</td><td headers="PHONE_NUMBER">515.124.4269</td><td headers="HIRE_DATE">28-SEP-05</td><td headers="JOB_ID">FI_ACCOUNT</td><td headers="SALARY">8200</td><td headers="COMMISSION_PCT"> - </td><td headers="MANAGER_ID">108</td><td headers="DEPARTMENT_ID">100</td></tr><tr><td headers="NUM">3</td><td headers="EMPLOYEE_ID">111</td><td headers="FIRST_NAME">Ismael</td><td headers="LAST_NAME">Sciarra</td><td headers="EMAIL">ISCIARRA</td><td headers="PHONE_NUMBER">515.124.4369</td><td headers="HIRE_DATE">30-SEP-05</td><td headers="JOB_ID">FI_ACCOUNT</td><td headers="SALARY">7700</td><td headers="COMMISSION_PCT"> - </td><td headers="MANAGER_ID">108</td><td headers="DEPARTMENT_ID">100</td></tr><tr><td headers="NUM">1</td><td headers="EMPLOYEE_ID">108</td><td headers="FIRST_NAME">Nancy</td><td headers="LAST_NAME">Greenberg</td><td headers="EMAIL">NGREENBE</td><td headers="PHONE_NUMBER">515.124.4569</td><td headers="HIRE_DATE">17-AUG-02</td><td headers="JOB_ID">FI_MGR</td><td headers="SALARY">12008</td><td headers="COMMISSION_PCT"> - </td><td headers="MANAGER_ID">101</td><td headers="DEPARTMENT_ID">100</td></tr>
{% for post in (1..12) %}
<td>...</td>
{% endfor %}
</tbody></table>
</div>


4\. Lastly combine steps 2 and 3 to consolidate the query and limit the results.

{% highlight sql %}
SELECT
   JOB_ID,
   LISTAGG(SALARY, ',') WITHIN GROUP (ORDER BY JOB_ID) "Salaries"
FROM (
SELECT * FROM (
    SELECT
      E.*,
      ROW_NUMBER() OVER(PARTITION BY JOB_ID ORDER BY JOB_ID) "NUM"
    FROM HR.EMPLOYEES E
    )
  WHERE NUM <= 3
)
GROUP BY JOB_ID;
{% endhighlight %}
<br />
<center>
<table summary="Results from Extended LISTAGG Query" class="u-Report">
<tbody><tr><th id="JOB_ID_226197420584683627420931559238450976648">JOB_ID</th><th id="Salaries_226197420584683627420931559238450976648">Salaries</th></tr><tr><td headers="JOB_ID">AC_ACCOUNT</td><td headers="Salaries">8300</td></tr><tr><td headers="JOB_ID">AC_MGR</td><td headers="Salaries">12008</td></tr><tr><td headers="JOB_ID">AD_ASST</td><td headers="Salaries">4400</td></tr><tr><td headers="JOB_ID">AD_PRES</td><td headers="Salaries">24000</td></tr><tr><td headers="JOB_ID">AD_VP</td><td headers="Salaries">17000,17000</td></tr><tr><td headers="JOB_ID">FI_ACCOUNT</td><td headers="Salaries">6900,7800,9000</td></tr><tr><td headers="JOB_ID">FI_MGR</td><td headers="Salaries">12008</td></tr><tr><td headers="JOB_ID">HR_REP</td><td headers="Salaries">6500</td></tr><tr><td headers="JOB_ID">IT_PROG</td><td headers="Salaries">4800,6000,9000</td></tr><tr><td headers="JOB_ID">MK_MAN</td><td headers="Salaries">13000</td></tr><tr><td headers="JOB_ID">MK_REP</td><td headers="Salaries">6000</td></tr><tr><td headers="JOB_ID">PR_REP</td><td headers="Salaries">10000</td></tr><tr><td headers="JOB_ID">PU_CLERK</td><td headers="Salaries">2500,2600,3100</td></tr><tr><td headers="JOB_ID">PU_MAN</td><td headers="Salaries">11000</td></tr><tr><td headers="JOB_ID">SA_MAN</td><td headers="Salaries">10500,11000,14000</td></tr><tr><td headers="JOB_ID">SA_REP</td><td headers="Salaries">10000,6200,7000</td></tr><tr><td headers="JOB_ID">SH_CLERK</td><td headers="Salaries">2600,2600,3200</td></tr><tr><td headers="JOB_ID">ST_CLERK</td><td headers="Salaries">2500,2600,3200</td></tr><tr><td headers="JOB_ID">ST_MAN</td><td headers="Salaries">7900,8000,8200</td></tr>
</tbody></table>
</center>
