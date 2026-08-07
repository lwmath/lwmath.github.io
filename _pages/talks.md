---
layout: archive
title: "Talks"
permalink: /talks/
author_profile: true
---

<!--
Fallback list:
If JavaScript is unavailable, all talks are still displayed here.
-->
<div id="talks-fallback">

  {% assign sorted_talks = site.data.talks | sort: "date" | reverse %}

  <ul>
    {% for talk in sorted_talks %}
      {% include talk-item.html talk=talk %}
    {% endfor %}
  </ul>

</div>


<!--
JavaScript-generated sections.
Hidden until the script has successfully classified the talks.
-->
<div id="talks-dynamic" hidden>

  <section id="upcoming-section" hidden>
    <h2>Upcoming Talks</h2>
    <ul id="upcoming-talks"></ul>
  </section>

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

  // --------------------------------------------------
  // 1. Get today's date in YYYY-MM-DD format
  // --------------------------------------------------

  const now = new Date();

  const year = now.getFullYear();
  const month = String(now.getMonth() + 1).padStart(2, "0");
  const day = String(now.getDate()).padStart(2, "0");

  const today = `${year}-${month}-${day}`;


  // --------------------------------------------------
  // 2. Read all talks from the fallback list
  // --------------------------------------------------

  const fallback = document.getElementById("talks-fallback");
  const dynamic = document.getElementById("talks-dynamic");

  if (!fallback || !dynamic) return;

  const talks = Array.from(
    fallback.querySelectorAll(".talk-item")
  );


  // --------------------------------------------------
  // 3. Classify talks
  // --------------------------------------------------

  const upcoming = [];
  const conferences = [];
  const seminars = [];

  talks.forEach(function (talk) {

    const startDate = talk.dataset.date;
    const endDate = talk.dataset.endDate || startDate;
    const category = talk.dataset.category;

    // A talk remains "upcoming" through its final day.
    if (endDate >= today) {
      upcoming.push(talk);
    }
    else if (category === "conference") {
      conferences.push(talk);
    }
    else {
      seminars.push(talk);
    }

  });


  // --------------------------------------------------
  // 4. Sort automatically
  // --------------------------------------------------

  // Upcoming talks:
  // nearest upcoming talk first
  upcoming.sort(function (a, b) {
    return a.dataset.date.localeCompare(b.dataset.date);
  });

  // Past talks:
  // newest first
  function sortPast(a, b) {
    return b.dataset.date.localeCompare(a.dataset.date);
  }

  conferences.sort(sortPast);
  seminars.sort(sortPast);


  // --------------------------------------------------
  // 5. Populate the three sections
  // --------------------------------------------------

  const upcomingList =
    document.getElementById("upcoming-talks");

  const conferenceList =
    document.getElementById("conference-talks");

  const seminarList =
    document.getElementById("seminar-talks");


  upcoming.forEach(function (talk) {
    upcomingList.appendChild(talk.cloneNode(true));
  });

  conferences.forEach(function (talk) {
    conferenceList.appendChild(talk.cloneNode(true));
  });

  seminars.forEach(function (talk) {
    seminarList.appendChild(talk.cloneNode(true));
  });


  // --------------------------------------------------
  // 6. Show only non-empty sections
  // --------------------------------------------------

  if (upcoming.length > 0) {
    document.getElementById("upcoming-section").hidden = false;
  }

  if (conferences.length > 0) {
    document.getElementById("conference-section").hidden = false;
  }

  if (seminars.length > 0) {
    document.getElementById("seminar-section").hidden = false;
  }


  // --------------------------------------------------
  // 7. Switch from fallback to dynamic layout
  // --------------------------------------------------

  dynamic.hidden = false;
  fallback.hidden = true;

});
</script>
