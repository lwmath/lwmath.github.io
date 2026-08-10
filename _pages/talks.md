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

  <section id="conference-section" hidden>
    <h2>Conference and Workshop Talks</h2>
    <div id="conference-talks"></div>
  </section>

  <section id="seminar-section" hidden>
    <h2>Seminar and Colloquium Talks</h2>
    <div id="seminar-talks"></div>
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

    const startDate = talk.dataset.date;
    const endDate = talk.dataset.endDate || startDate;
    const category = talk.dataset.category;

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

  conferences.sort(function (a, b) {
    return b.dataset.date.localeCompare(a.dataset.date);
  });

  seminars.sort(function (a, b) {
    return b.dataset.date.localeCompare(a.dataset.date);
  });

  const conferenceContainer =
    document.getElementById("conference-talks");

  const seminarContainer =
    document.getElementById("seminar-talks");

  function renderByYear(talkArray, container) {

    const groups = {};

    talkArray.forEach(function (talk) {

      const talkYear =
        talk.dataset.date.slice(0, 4);

      if (!groups[talkYear]) {
        groups[talkYear] = [];
      }

      groups[talkYear].push(talk);

    });

    const years =
      Object.keys(groups);

    years.sort(function (a, b) {
      return b.localeCompare(a);
    });

    years.forEach(function (talkYear) {

      const group =
        document.createElement("div");

      group.className =
        "talk-year-group";

      const heading =
        document.createElement("h3");

      heading.className =
        "talk-year";

      heading.textContent =
        talkYear;

      const list =
        document.createElement("ul");

      list.className =
        "talk-year-list";

      groups[talkYear].forEach(function (talk) {
        list.appendChild(
          talk.cloneNode(true)
        );
      });

      group.appendChild(heading);
      group.appendChild(list);

      container.appendChild(group);

    });

  }

  renderByYear(
    conferences,
    conferenceContainer
  );

  renderByYear(
    seminars,
    seminarContainer
  );

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
