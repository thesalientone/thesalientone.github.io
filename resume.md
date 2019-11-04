---
layout : default
title : Resume
---

# Stefan Jenkins #
----

## Objective ##
To secure an entry level position as a software engineer or data analyst with opportunity for growth.

---
## Technical Skills ##
<center>
  <table>
    <tr>

      <td>
        <strong>Proficient</strong>
      </td>
      <td>
        <strong>Working Knowledge</strong>
      </td>
      <td>
        <strong>Learning</strong>
      </td>

    </tr>
    <tr>
      <td>
        <ul>
          <li>Python</li>
          <ul>
            <li>Numpy</li>
            <li>OpenCV</li>
            <li>Tkinter</li>
            <li>Pandas</li>
          </ul>
          <li>HTML/CSS</li>
          <li>Visual Basic for Applications</li>
          <li>Minitab</li>
          <li>Matlab</li>
        </ul>
      </td>
      <td>
        <ul>
          <li>C Programming</li>
          <li>Adobe CS</li>
          <li>Visual Basic</li>
          <li>SQL</li>
          <li>Javascript </li>
          <li>Ruby </li>
          <li>R</li>
          <li>Command Line</li>
          <li>Java </li>
          <li>Git</li>
          <li>AWS</li>
        </ul>
      </td>
      <td>
        <ul>
          <li>Ruby on Rails</li>
          <li>Django</li>
          <li>Tensorflow</li>
        </ul>
      </td>
    </tr>
  </table>
</center>
---

## Experience ##
{% assign j = site.jobs | sort: 'end_date' | reverse %}
{% for job in j %}
### {{job.name}} ###
{% if job.short_name == 'maxxis' %}
#### {{job.start_date | date: "%b %Y"}} ~ Present ####
{% else %}
#### {{job.start_date | date: "%b %Y"}} ~ {{job.end_date | date: "%b %Y"}} ####
{% endif %}
#### {{job.position}} ####
{{job.content}}
{% endfor %}
----
## Education ##

### Colorado Technical University ###

<div>
 <div class="p-left">B.S. IT - Systems Software Engineering</div>
 <div class="p-right">May 2017 - May 2020</div>
</div>

<br>
### Florida A&M University ###

<div>
 <div class="p-left">Chemical/Biomedical Engineering</div>
 <div class="p-right">Aug 2008 - Dec 2014</div>
</div>
<br>
<br>

----
## Awards ##

### AAAS First Place, Engineering 2009 ###
#### Research on Spherical Projections in Porous Carbon Foams ####

During the summer of 2009 I had an internship where I did research on porous carbon foams
at the National High Magnetic Field Laboratory in Tallahassee, FL. My work
focused on acquiring SEM images of the foam and determining the porosity and average pore size. I discovered
a formula that determines the 3D radius of a foam bubble from a 2D picture. I received
this award for presenting my research at a AAAS conference.

### HCASC, National Champions 2011 ###
#### Academic competition with $100,000 grand prize ####

Honda hosts a national academic competition called the Honda Campus All Star Challenge
(HCASC) every year between 64 HBCUs. Students compete while answering questions on a wide variety of topics.
I competed all four years of undergraduate school, and during my junior year we won the grand prize of $100,000. My specialty
areas were science, mathematics, humanities, and popular culture.

<br>

----

### Organizations ###
NSBE, AIChE, BMES, Presidential Scholars Association, Honors Student Association, HCASC,
Vienna Boys Choir
