---
layout : default
title : Resume
---
# Stefan Jenkins #
----
## Objective ##
To contribute to the success of a company by applying my expert data engineering and Python development skills and
experience to enhance their business functions and performance.

---
## Technical Skills ##

**Programming Languages**: Python, SQL , Java, JavaScript, Ruby, HTML / CSS, C Programming, Visual Basic for
Applications, Markdown, Visual Basic, RegEx, REST APIs

**Databases**: Oracle, MongoDB, BigQuery, MySql

**Applications**: Kofax Kapow RPA, AWS, Google Cloud Platform, Tableau, Minitab, Matlab, R, Git, Adobe CS, Jira

**Libraries**: Numpy, OpenCV, Tkinter, Pandas, BeautifulSoup, jQuery, Django, TensorFlow

---

## Experience ##

{% assign j = site.jobs | sort: 'end_date' | reverse %}


{% for job in j %}
### {{job.name}} ###
{% if job.short_name == 'none' %}
#### {{job.start_date | date: "%b %Y"}} - Present ####
{% else %}
#### {{job.start_date | date: "%b %Y"}} - {{job.end_date | date: "%b %Y"}} ####
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
National Society of Black Engineers, American Institute of Chemical Engineers, Biomedical Engineering Society, Presidential Scholars Association, Honors Student Association, Honda Campus All Star Challenge,
Vienna Boys Choir
