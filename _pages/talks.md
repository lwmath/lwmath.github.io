---
layout: archive
title: "Talks"
permalink: /talks/
author_profile: true
---

<div id="talks-fallback">

  {% assign sorted_talks = site.data.talks | sort: "date" | reverse %}

  <ul>
    {% for talk in sorted_talks %}
      {% include talk-item.html talk=talk %}
    {% endfor %}
  </ul>

</div>


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

  const now = new Date();

  const year = now.getFullYear();
  const month = String(now.getMonth() + 1).padStart(2, "0");
  const day = String(now.getDate()).padStart(2, "0");

  const today = `${year}-${month}-${day}`;

  const fallback = document.getElementById("talks-fallback");
  const dynamic = document.getElementById("talks-dynamic");

  if (!fallback || !dynamic) {
    return;
  }

  const talks = Array.from(
    fallback.querySelectorAll(".talk-item")
  );

  const upcoming = [];
  const conferences = [];
  const seminars = [];

  talks.forEach(function (talk) {

    const startDate = talk.dataset.date;
    const endDate = talk.dataset.endDate || startDate;
    const category = talk.dataset.category;

    if (endDate >= today) {
      upcoming.push(talk);
    }
    else if (category === "conference") {
      conferences.push(talk);
    }
    else if (category === "seminar") {
      seminars.push(talk);
    }

  });

  upcoming.sort(function (a, b) {
    return a.dataset.date.localeCompare(b.dataset.date);
  });

  conferences.sort(function (a, b) {
    return b.dataset.date.localeCompare(a.dataset.date);
  });

  seminars.sort(function (a, b) {
    return b.dataset.date.localeCompare(a.dataset.date);
  });

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

  if (upcoming.length > 0) {
    document.getElementById("upcoming-section").hidden = false;
  }

  if (conferences.length > 0) {
    document.getElementById("conference-section").hidden = false;
  }

  if (seminars.length > 0) {
    document.getElementById("seminar-section").hidden = false;
  }

  dynamic.hidden = false;
  fallback.hidden = true;

});
</script>
