---
layout : default
title : Resume
---

# Stefan Jenkins #
----
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
 <div class="p-left">B.S. IT - Chemical/Biomedical Engineering</div>
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

Honda hosts a national competition called the Honda Campus All Star Challenge
(HCASC) every year between 64 HBCUs. I competed all four years of undergraduate school,
and during my junior year we won the grand prize of $100,000. My specialties
areas were science, mathematics, humanities, and popular culture.

<br>

----
## Skills ##

### Languages ###
Python (Numpy, OpenCV, Tkinter, Pandas, Tensorflow, Django), Ruby (Ruby on Rails),
Visual Basic for Applications, C, SQL, Javascript (JQuery), R, Java, Bash
HTML/CSS

### Software ###
Matlab, Unix/Linux, Windows, OSX, MS Office Suite, MS Project, Minitab, Adobe CS, Git

### Competencies ###
Communication, Command Line, Web Development, GUI Development, 100WPM, 10 key, Project Management, Problem Solving,
Calculus, Differential Equations, Statistics, Regression Analysis, Design of Experiment,
 Numerical Analysis, Monte-Carlo Simulation, Statistical Process Control,  Piano

### Organizations ###
NSBE, AIChE, BMES, Presidential Scholars Association, Honors Student Association, HCASC,
Vienna Boys Choir
