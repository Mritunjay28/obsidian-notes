---
layout: default
title: Topics
permalink: /topics/
bare: true
---
<div class="home topics-page">
<p><a class="back-link" href="{{ '/' | relative_url }}">&larr; All subjects</a></p>
<h1 id="topics-title">Topics</h1>
<div id="topics-list"></div>
{% include notes-index.html %}
<script>
(function () {
  var el = document.getElementById('notes-index');
  if (!el) return;
  var notes = JSON.parse(el.textContent);
  var subOrder = [];
  notes.forEach(function (n) { if (n.s && subOrder.indexOf(n.s) < 0) subOrder.push(n.s); });
  var g = new URLSearchParams(location.search).get('g');
  var nat = function (a, b) {
    var sa = a.s || '', sb = b.s || '';
    if (sa !== sb) { var ia = subOrder.indexOf(sa), ib = subOrder.indexOf(sb); return (ia < 0 ? 99 : ia) - (ib < 0 ? 99 : ib); }
    return a.t.localeCompare(b.t, undefined, { numeric: true, sensitivity: 'base' });
  };
  var subLabel = function (s) { return g && s && s.indexOf(g + ' ') === 0 ? s.slice(g.length + 1) : s; };
  var list = notes.filter(function (n) { return n.g === g; }).sort(nat);
  var title = document.getElementById('topics-title');
  var root = document.getElementById('topics-list');
  if (!g || !list.length) {
    title.textContent = 'All subjects';
    var groups = {};
    notes.forEach(function (n) { groups[n.g] = (groups[n.g] || 0) + 1; });
    Object.keys(groups).forEach(function (name) {
      var a = document.createElement('a');
      a.className = 'topic-row';
      a.href = {{ "/topics/" | relative_url | jsonify }} + '?g=' + encodeURIComponent(name);
      a.textContent = name + ' (' + groups[name] + ')';
      root.appendChild(a);
    });
    return;
  }
  title.textContent = g;
  var lastSub = null, ul = null;
  list.forEach(function (n) {
    if (n.s !== lastSub) {
      lastSub = n.s;
      if (n.s) { var h = document.createElement('h3'); h.className = 'home-sub'; h.textContent = subLabel(n.s); root.appendChild(h); }
      ul = document.createElement('ul'); ul.className = 'home-list'; root.appendChild(ul);
    }
    var li = document.createElement('li');
    var a = document.createElement('a'); a.href = n.u; a.textContent = n.t;
    li.appendChild(a); ul.appendChild(li);
  });
})();
</script>
</div>
