---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
classes: wide
---

{% include base_path %}
{% assign cv_url = site.baseurl | append: "/files/CV_academic_2026.pdf" %}

<p class="cv-actions">
  <a class="btn btn--primary" href="{{ cv_url }}" target="_blank" rel="noopener">
    <i class="fas fa-file-pdf" aria-hidden="true"></i> Open / Download CV (PDF)
  </a>
</p>

<!-- Desktop: browser's built-in PDF viewer -->
<iframe class="cv-frame" src="{{ cv_url }}" title="Curriculum Vitae of Chun-Yi Chang" width="100%" height="900px"></iframe>

<!-- Phones: every page rendered to fit the screen width -->
<div id="cv-pages" class="cv-pages" aria-label="Curriculum Vitae of Chun-Yi Chang"></div>

<style>
  .cv-frame { border: 1px solid #ddd; }
  .cv-pages { display: none; }
  .cv-pages canvas {
    display: block;
    width: 100%;
    height: auto;
    margin: 0 0 1em;
    border: 1px solid #ddd;
    background: #fff;
  }
  @media (max-width: 768px) {
    .cv-frame { display: none; }
    .cv-pages { display: block; }
  }
</style>

<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<script>
  (function () {
    var container = document.getElementById("cv-pages");
    if (!window.matchMedia("(max-width: 768px)").matches) return;
    if (!window.pdfjsLib) {
      container.innerHTML = '<p>Your browser could not display the CV here. Please use the button above to open the PDF.</p>';
      return;
    }
    pdfjsLib.GlobalWorkerOptions.workerSrc =
      "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";
    pdfjsLib.getDocument("{{ cv_url }}").promise.then(function (pdf) {
      var chain = Promise.resolve();
      for (var n = 1; n <= pdf.numPages; n++) {
        (function (num) {
          chain = chain.then(function () {
            return pdf.getPage(num).then(function (page) {
              var ratio = Math.min(window.devicePixelRatio || 1, 3);
              var cssWidth = container.clientWidth || window.innerWidth;
              var base = page.getViewport({ scale: 1 });
              var viewport = page.getViewport({ scale: (cssWidth / base.width) * ratio });
              var canvas = document.createElement("canvas");
              canvas.width = viewport.width;
              canvas.height = viewport.height;
              canvas.setAttribute("aria-label", "CV page " + num);
              container.appendChild(canvas);
              return page.render({ canvasContext: canvas.getContext("2d"), viewport: viewport }).promise;
            });
          });
        })(n);
      }
      return chain;
    }).catch(function () {
      container.innerHTML = '<p>Your browser could not display the CV here. Please use the button above to open the PDF.</p>';
    });
  })();
</script>
