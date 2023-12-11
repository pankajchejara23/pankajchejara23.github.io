---
title: "Projects"
about:
  template: solana
format:
    html: 
      self-contained: true
      grid: 
        margin-width: 350px
toc: true
---

## EdTech Projects

The following projects focus on utilizing the power of analytics to support teaching and learning experience.



### Speaking participation detection using Raspberry Pi

[Demo](https://www.youtube.com/watch?v=9xmeXMp7Hp8) | [Publication](https://ceur-ws.org/Vol-2610/paper3.pdf) | [Source code](https://github.com/pankajchejara23/CoTrack) 

`Python`, `Dash`

* A Raspberry Pi-based prototype to capture audio data from face-to-face group activity
* Analyzed the direction of arrival of captured audio to compute speaking time and turn-taking using python
* Developed an interactive dashboard for data visualization

<br/><br/>



### CoTrack - Web application for monitoring collaboration behavior in classrooms

🏆 **Best Demo Award** at Learning Analytics and Knowledge (LAK2023) conference, Texas, USA (Core A rank)

[Demo](https://youtu.be/l-B2hXGRvek) | [Publication](https://ceur-ws.org/Vol-2610/paper3.pdf) | [Source code](https://github.com/pankajchejara23/CoTrackv2)

`Python` , `jQuery`, `MySQL`, `Etherpad`, `plotly`, `Google Speech-to-Text`

* Python Django app to collect multimodal learning data from group activities in classrooms

* Voice activity detection to compute speaking time of individual in group activities in real-time

* Google Speech-To-Text integration to process audio in real-time

* A real-time dashboard visualizing group's writing behavior, speaking participation, and speech content (in the form of word cloud)

<br/>

<br/>

### Temporal window size identification for developing collaboration estimation model

 [Publication](https://www.researchgate.net/profile/Pankaj-Chejara/publication/366006788_Impact_of_window_size_on_the_generalizability_of_collaboration_quality_estimation_models_developed_using_Multimodal_Learning_Analytics/links/638dc4cb2c563722f23d332e/Impact-of-window-size-on-the-generalizability-of-collaboration-quality-estimation-models-developed-using-Multimodal-Learning-Analytics.pdf) | [Source code](https://github.com/pankajchejara23/Time-window-size-impact-on-model-performance)

`Python` , `Scikit-learn`, `Matplotlib` 

* Analyzed audio and log data gathered from Estonian classrooms during group activities

* Explored use of different temporal window size (30s, 60s, 90s, 120s, 180s, 240s) to process features such as speaking time, turn-taking

* Developed machine learning models to predict collaboration quality using audio and log data features processed with different window sizes

<br/>

<br/>

### Across schools generalizability evaluation of machine learning models to predict collaboration quality

[Publication](https://bera-journals.onlinelibrary.wiley.com/doi/abs/10.1111/bjet.13402) 

`Python` , `Scikit-learn`, `Matplotlib`

* Analyzed audio, video and log data to develop machine learning models to predict collaboration quality in classroom settings during group activity

* Used Random Forest algorithm to develop model and evaluated its generalizability with a different dataset collected from a different Estonian school


## Web-Development Projects

I worked on the following projects as part of my web-developer role at Tallinn University.

### TruestedUx

[Website](https://www.trustux.org/) 

`Python` , `Django`, `jQuery`, `Bootstrap`

* An application which allows assessment of trust aspect of technology usage. 

* Developed the complete front-end part and the most of back-end of the website.
 

### Tinda

[Website](https://web.htk.tlu.ee/tinda/et/user/login) 

`Drupal` ,  `jQuery`, `Highcharts`

* An application which allows assessment of digital competencies of professionals.

* Added an module to download the responses of users to questionnaires on digital competencies following the [DigiComp](https://digital-skills-jobs.europa.eu/en/actions/european-initiatives/digital-competence-framework-digcomp) Framework.

* Added a dashboard visualizing users' responses and their overall score for digital competencies. 

* Added a report download functionality.


### Seeds

[Website](http://app.seeds-project.org/en/) 

`Python` , `Django`, `jQuery`, `Bootstrap`, `Plotly`

* A map-based application to allows users to filter energy transition scenario based on their preferences.

* Handled the entire development of the project including front-end and back-end.