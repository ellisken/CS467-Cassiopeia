#  Introduction
The Cassiopeia web application provides an innovative learning model for global language learners. It features a robust learning methodology that incorporates repeat exposure to material, self-assessment, and curated content generated according to an individual user’s learning curve. On sign-up, a user selects the language they wish to learn, their self-assessed skill level, and topics of interest, such as sports, culture, or politics. When signed in, the user home page displays a list of articles for the user to peruse and rate that match the user’s selected language and categories of interest.The application leverages NLP technology in the background to periodically assess a user’s foreign language reading level based on user-provided ratings, then matches the reading level with the user’s interests to push the most suitable material to that user. As that user continues to read and rate articles, the app develops a deeper understanding of exactly which material is right for that user, facilitating a fun and engaging learning experience.

The Cassiopeia platform currently supports English as a foreign language with plans to support additional languages in the future. Likewise, content consists of news articles only but will be expanded to include video as the project progresses.

# System Architecture and Software

## APIs
Cassiopeia Core API
EventRegistry API - crawls the web for articles and stores articles in the Content table of the Cassiopeia database
Libraries
Python NLTK - used to implement the Naive Bayes classifier
Bootstrap
JQuery
Vue.js
Back End
Web Server 
Google App Engine
Flask
Nginx
Gunicorn
Pyenv - Python 3.0
Database
Google MySQL
Flask SQLAlchemy for data modeling and database interfacing
Front end
HTML/JS/CSS/VUE/JINJA
Development Tools
Github
Google Cloud

Cassiopeia is built on the Flask framework using Python as the main backend programming language and Flask SQLAlchemy for interfacing with the app’s database. Built with scalability in mind, the Google Cloud Platform hosts both the app engine and MySql database. Instead of connecting users to a standalone server, Cassiopeia uses distributed instances to load balance. This provides the app the ability to scale horizontally as its user base and traffic grow. Each instance of the app handles the core business logic by receiving requests from clients and serving HTML. The front end leverages Vue.js and Jinja templates.

The Cassiopeia Restful API connects the back end to the front end. It routes queries and sends articles based on the user setting and filters on content appropriate to the user skill levels. The API also relays the user and pagination information to help drive front-end experience.

In the back end, the app engine exchanges information with the database. The database connects to a standalone NLP engine. In production, users have no direct communication with the NLP engine, but for the demonstration purposes, we exposed the NLP training and classifier functions in the front end. The NLP engine has two main functions: 1) setting a baseline difficulty score on all new content, and 2) training and/or updating individual users’ reading level classifiers and feature sets. 

Baseline difficulty scores are calculated using the Automated Readability Index (ARI). The ARI was developed in the 1960s by Prof. Senter and Prof. Smith for the US military. It is still widely used today in the language industry as a heuristic formula developed to assess the understandability or readability of an English text:


The ARI produces scores in the range of 1-14, where 1 corresponds to Kindergarten and 14 to College. Cassiopeia translates ARI scores by mapping scores 1 ~ 5 to “easy,” 5 ~ 9 to “intermediate,” and 9 ~ 14 to “advanced.”

Another key part of Cassiopeia’s NLP engine is Naive Bayes classification. We follow the standard Naive Bayes algorithm for finding the posterior probability of a given article for each of three classes: too easy, just right, and too difficult (same as how users would rate them).
 
This occurs by establishing a list of all words and their associated categories from all articles a user has rated, building a frequency table (by category) of those words, and using the frequencies to create a likelihood that an article containing some of those feature words belongs to either the too easy, just right, or too difficult category. 

In plain terms, the classifier assigns a category to an article based on that article’s vocabulary. 

Consider the following example:
A user rates some articles as “too difficult”. Words “supercalifragilisticexpialidocious,” “Pneumonoultramicroscopicsilicovolcanoconiosis,” and “Antidisestablishmentarianism” occur with high frequency in these articles.
A classifier is created when the model is trained.
When analyzing new articles, the classifier classifies a articles with a high frequency of these crazy long words as “too difficult.”
The user’s level is re-calculated with the standard deviation of all the articles classified as “just right”.

While the NLP engine’s vocabulary-based approach to assigning reading difficulty differs from the ARI’s consideration of sentence, character, and word count, it provides a rudimentary machine learning foundation upon which to build.

# References and Sources
References
http://flask.pocoo.org/docs/0.12/quickstart/
http://flask-sqlalchemy.pocoo.org/2.3/
http://flask-sqlalchemy.pocoo.org/2.3/queries/
http://flask-bcrypt.readthedocs.io/en/latest/
http://flask-login.readthedocs.io/en/latest/
http://flask-wtf.readthedocs.io/en/stable/
https://youtu.be/MwZwr5Tvyxo
https://www.englishcentral.com/terms
https://vuejs.org/
https://en.wikipedia.org/wiki/Automated_readability_index
https://en.wikipedia.org/wiki/Naive_Bayes_classifier
https://www.analyticsvidhya.com/blog/2017/09/naive-bayes-explained/
https://en.wikipedia.org/wiki/Posterior_probability


Sources for Images and other files
https://restcountries.eu/#api-endpoints-language - Country Images
https://docs.oracle.com/cd/E13214_01/wli/docs92/xref/xqisocodes.html - Country codes
https://www.loc.gov/standards/iso639-2/php/code_list.php - Language codes
https://eventregistry.org/ - Articles
https://mobirise.com/ - CSS templates and images for About page
https://getbootstrap.com/docs/4.1/examples/sign-in/ - CSS templates for Home, Login and Signup pages
https://www.brandeps.com/logo/C/Capistrano-01 - Site logo
https://www.bespeaking.com/root-word-err/ - Banner 1
https://www.linkedin.com/pulse/taking-alumni-engagement-next-level-metrics-joe-volin-schipke/ - Banner 2
https://blog.xploresports.com/ - Banner 3
https://www.flaticon.com/categories/web - Category icons
