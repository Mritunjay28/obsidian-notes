---
layout: default
title: Home
---
<div class="home">
<h1>My Notes</h1>
<p class="home-hint">Hover a card to preview its topics, or open it for the full list.</p>
<div id="card-grid" class="card-grid"></div>
{% include notes-index.html %}
<script>
(function () {
  var el = document.getElementById('notes-index');
  var grid = document.getElementById('card-grid');
  if (!el || !grid) return;
  var notes = JSON.parse(el.textContent);
  var order = ["DSA", "OOPs", "DBMS", "SQL", "System Design"];
  var groups = {};
  notes.forEach(function (n) { (groups[n.g] = groups[n.g] || []).push(n); });
  var subOrder = [];
  notes.forEach(function (n) { if (n.s && subOrder.indexOf(n.s) < 0) subOrder.push(n.s); });
  var names = Object.keys(groups).sort(function (a, b) {
    var ia = order.indexOf(a), ib = order.indexOf(b);
    if (ia < 0 && ib < 0) { if (a === "Others") return 1; if (b === "Others") return -1; return a.localeCompare(b); }
    if (ia < 0) return 1; if (ib < 0) return -1; return ia - ib;
  });
  var nat = function (a, b) {
    var sa = a.s || '', sb = b.s || '';
    if (sa !== sb) { var ia = subOrder.indexOf(sa), ib = subOrder.indexOf(sb); return (ia < 0 ? 99 : ia) - (ib < 0 ? 99 : ib); }
    return a.t.localeCompare(b.t, undefined, { numeric: true, sensitivity: 'base' });
  };
  var subLabel = function (g, s) { return s && s.indexOf(g + ' ') === 0 ? s.slice(g.length + 1) : s; };
  names.forEach(function (g) {
    var list = groups[g].sort(nat);
    var a = document.createElement('a');
    a.className = 'card';
    a.href = {{ "/topics/" | relative_url | jsonify }} + '?g=' + encodeURIComponent(g);
    var h = document.createElement('h3'); h.textContent = g;
    var c = document.createElement('span'); c.className = 'count';
    c.textContent = list.length + (list.length === 1 ? ' note' : ' notes');
    var prev = document.createElement('div'); prev.className = 'preview';
    var lastSub = null;
    list.forEach(function (n) {
      if (n.s !== lastSub) {
        lastSub = n.s;
        if (n.s) { var ps = document.createElement('div'); ps.className = 'psub'; ps.textContent = subLabel(g, n.s); prev.appendChild(ps); }
      }
      var pl = document.createElement('span'); pl.className = 'plink'; pl.textContent = n.t; prev.appendChild(pl);
    });
    a.appendChild(h); a.appendChild(c); a.appendChild(prev);
    grid.appendChild(a);
  });
})();
</script>
</div>
