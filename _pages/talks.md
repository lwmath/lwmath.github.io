---
layout: archive
title: "Talks"
permalink: /talks/
author_profile: true
---


<style>

.talk-item {
  margin-bottom: 1.15rem;
  line-height: 1.5;
}

.talk-title {
  margin-bottom: 0.12rem;
  line-height: 1.4;
}

.talk-details {
  line-height: 1.55;
}

#talks-dynamic section {
  margin-bottom: 2.5rem;
}

#talks-dynamic section > h2 {
  margin-top: 1.8rem;
  margin-bottom: 1.2rem;
}


@media (max-width: 600px) {

  .talk-item {
    margin-bottom: 1rem;
  }

}

</style>


<div id="talks-fallback">

  {% assign sorted_talks = site.data.talks | sort: "date" | reverse %}

  <ul>
    {% for talk in sorted_talks %}
      {% include talk-item.html talk=talk %}
    {% endfor %}
  </ul>

</div>


<div id="talks-dynamic" hidden>


  <section id="conference-section" hidden>

    <h2>Conference and Workshop Talks</h2>

    <ul id="conference-talks"></ul>

  </section>


  <section id="seminar-section" hidden>

    <h2>Seminar and Colloquium Talks</h2>

    <ul id="seminar-talks"></ul>

  </section>


</div>



<script>

document.addEventListener("DOMContentLoaded", function () {


  const now = new Date();

  const year = now.getFullYear();
  const month = String(now.getMonth() + 1).padStart(2, "0");
  const day = String(now.getDate()).padStart(2, "0");

  const today = `${year}-${month}-${day}`;



  const fallback =
    document.getElementById("talks-fallback");


  const dynamic =
    document.getElementById("talks-dynamic");


  if (!fallback || !dynamic) {

    return;

  }



  const talks = Array.from(
    fallback.querySelectorAll(".talk-item")
  );



  const conferences = [];

  const seminars = [];



  talks.forEach(function (talk) {


    const endDate =
      talk.dataset.endDate || talk.dataset.date;


    const category =
      talk.dataset.category;



    /*
       Remove all future talks.
       They are displayed on the homepage.
    */

    if (endDate >= today) {

      return;

    }



    if (category === "conference") {

      conferences.push(talk);

    }


    else if (category === "seminar") {

      seminars.push(talk);

    }


  });



  /*
     Newest talks first.
  */

  conferences.sort(function (a, b) {

    return b.dataset.date.localeCompare(
      a.dataset.date
    );

  });



  seminars.sort(function (a, b) {

    return b.dataset.date.localeCompare(
      a.dataset.date
    );

  });



  const conferenceList =
    document.getElementById("conference-talks");


  const seminarList =
    document.getElementById("seminar-talks");



  conferences.forEach(function (talk) {

    conferenceList.appendChild(
      talk.cloneNode(true)
    );

  });



  seminars.forEach(function (talk) {

    seminarList.appendChild(
      talk.cloneNode(true)
    );

  });



  if (conferences.length > 0) {

    document.getElementById(
      "conference-section"
    ).hidden = false;

  }



  if (seminars.length > 0) {

    document.getElementById(
      "seminar-section"
    ).hidden = false;

  }



  dynamic.hidden = false;

  fallback.hidden = true;



});

</script>
