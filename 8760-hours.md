---
layout: default
title: 8,760 Hours
description: Every hour of 2018, mapped and color-coded, recovered from a spreadsheet I kept when I was 16.
---

## 8,760 Hours

*June 2026*

When I was 16 I decided to log what I was doing, every single hour of every single day, for an entire year: one cell per hour in a Google Sheet, sorted into twelve color coded categories. I recently dug 2018 back out of Google Drive. Here it is, 365 days times 24 hours, all 8,760 of them. Scroll down to go through the year.

<style>
.vida-legend {
  display: flex;
  flex-wrap: wrap;
  gap: 0.35rem 1.1rem;
  font-size: 0.8rem;
  margin: 1.2rem 0 1.2rem 0;
  justify-content: center;
}
.vida-legend-item {
  display: flex;
  align-items: center;
  gap: 0.35rem;
  white-space: nowrap;
}
.vida-swatch {
  display: inline-block;
  width: 12px;
  height: 12px;
  border-radius: 2px;
  flex-shrink: 0;
}
.vida-row {
  display: flex;
  gap: 0.6rem;
  align-items: stretch;
  margin: 1.5rem 0 0.5rem 0;
}
.vida-monthcol {
  position: relative;
  width: 2.4rem;
  flex-shrink: 0;
  font-size: 0.65rem;
  color: #999;
}
.vida-monthcol span {
  position: absolute;
  left: 0;
}
.vida-gridcol {
  flex: 1 1 0;
  min-width: 0;
}
.vida-hourlabel-row {
  display: flex;
  justify-content: space-between;
  font-size: 0.65rem;
  color: #999;
  margin-bottom: 0.25rem;
}
#vida-grid {
  display: block;
  width: 100%;
  height: auto;
  image-rendering: pixelated;
  image-rendering: crisp-edges;
  border-radius: 4px;
}
.vida-commentscol {
  position: relative;
  flex: 1 1 0;
  min-width: 0;
  font-size: 0.8rem;
  line-height: 1.5;
}
.vida-comment {
  position: absolute;
  left: 0;
  right: 0;
}
.vida-comment b {
  color: #4a5a4f;
}
@media (max-width: 600px) {
  .vida-row {
    display: block;
  }
  .vida-monthcol {
    display: none;
  }
  .vida-comment {
    position: static;
    margin: 0.8rem 0;
  }
}
</style>

<div class="vida-legend">
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(225, 45%, 28%)"></span>Sleep</div>
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(25, 85%, 58%)"></span>Games</div>
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(35, 25%, 58%)"></span>School</div>
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(195, 60%, 78%)"></span>Relax</div>
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(340, 65%, 68%)"></span>Social</div>
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(150, 25%, 48%)"></span>Productive</div>
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(48, 75%, 62%)"></span>Family</div>
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(5, 70%, 52%)"></span>Travel</div>
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(0, 0%, 80%)"></span>Junk</div>
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(175, 40%, 42%)"></span>Extracurricular</div>
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(280, 40%, 62%)"></span>Creative</div>
  <div class="vida-legend-item"><span class="vida-swatch" style="background:hsl(100, 50%, 46%)"></span>Exercise</div>
</div>

<div class="vida-row">
  <div class="vida-monthcol">
    <span style="top:0.00%">Jan</span>
    <span style="top:8.49%">Feb</span>
    <span style="top:16.16%">Mar</span>
    <span style="top:24.66%">Apr</span>
    <span style="top:32.88%">May</span>
    <span style="top:41.37%">Jun</span>
    <span style="top:49.59%">Jul</span>
    <span style="top:58.08%">Aug</span>
    <span style="top:66.58%">Sep</span>
    <span style="top:74.79%">Oct</span>
    <span style="top:83.29%">Nov</span>
    <span style="top:91.51%">Dec</span>
  </div>
  <div class="vida-gridcol">
    <div class="vida-hourlabel-row">
      <span>00:00</span>
      <span>23:00</span>
    </div>
    <canvas id="vida-grid" width="336" height="2190" role="img" aria-label="Grid of 8,760 hourly activity blocks for 2018, one row per day and one column per hour, color coded by activity"></canvas>
  </div>
  <div class="vida-commentscol">
    <div class="vida-comment" style="top:8%"><b>Early February.</b> Summer vacation. I woke up at 11:20am on average that month, and on Feb 6 I logged 14 of 24 hours as &quot;play,&quot; my single biggest day of the year for that category.</div>
    <div class="vida-comment" style="top:16%"><b>March.</b> School starts, and you can watch the tan column light up here and stay lit for most of the rest of the year. You can also watch my bedtime creep earlier in real time. In January and February there is a 0% chance I am asleep by midnight. By June, deep into the school year, that number is 87% (97% by 1am). July, winter break, loosens it up again, and by December summer vacation has reset the clock completely.</div>
    <div class="vida-comment" style="top:65%"><b>Late August into September: Japan.</b> You can see exactly where. The dark blue sleep block jumps from the left edge of the row to the middle and stays there for two weeks. Buenos Aires and Tokyo are 12 hours apart, almost exactly half a day, so my schedule did not shift, it flipped: from sleeping roughly midnight to 8am, to sleeping roughly 10am to 7pm, within about a day of landing. The travel days on each end (Aug 19, 20, Sep 3, 4) all show zero hours of sleep. Four days after getting home, on Sep 8, I logged 16 hours of sleep, my biggest sleep day of the year, paying the whole jet lag debt off in one go.</div>
    <div class="vida-comment" style="top:95%"><b>December 31st.</b> 365 rows, 365 days, all fully filled in. 16 year old me did not miss a single hour all year.</div>
  </div>
</div>

By the numbers: 3,151 hours asleep (about 8.6 a night, 36% of the year), 1,931 hours of games versus 267 hours of "productive" time (about 7 to 1, or 22% versus 3%), 39 hours of exercise for the entire year (about 6 minutes a day), and 224 hours filed under "basura," which is Spanish for trash.

<script>
(function () {
  var data = "fjddddddddbpprjfbrrjjvfvjjdddddddddppbvvvvvvvjjjjjddddddddjppjjjffffbjjjjddddddddjjjjpjkvbpvkjjjrdddddddddkjjjjjjjjfjjjjrrdddddddddjjfjpkjjrjjfjrrddddddddbpfffffpjjjjjrrrddddddddrrrrjjjpppbjjrrddddddddddjjpjjkpbkffffrrrddddddddjkkjjjrrjjkssrrddddbvvvvbssrrrbrsrrrrrrddddddddbxrsxsrrbpsrrrrdddddddddbprrrrvsfrvjjjjjddddddddbjxssssssssxbfvbrddddddddprrrsrprbvssssvddddddpbfvvsssssrsrsrsrdddddddddbsxxrssbssbrssrddddddddbrsvsrrssrsbrsrbdddddddddbbssrsssrbpbsssdddddddddrrbprsrsssbsssrbddddddddbxrsrsxxrbpbrrssddddddddpbrrssrrsbxrrsrsddddddddbrsxrssprbssssjrdddddddddpprsssbrsbrsbbrddddddddpbsrrssrssbprssrddddddddddbrrsssxbbrrssssdddddddddpsrrssrrsbsssssddddddddpbrssrssbvvvvvvrrdddddddpkjbppjrjjrrrrrdddddddddbkrrrrrrrrrjjjdddddddddddbjjjpvpvjjjrrdddddddddjjjppjppjrjjjrrddddddddddrpjjjvpkjjjjjjppdddddddkjrffffffjjjjjjrdddddddddjjjkjjrrvjjjjrdddddddddjjjfffjpjjjjjjjjdddddddddpjjjjjjjjjjjjrdddddddddddjjkppjjjjjjjjrrddddddddjjjrjjjjjrjjjjrddddddddkjjjkjjjjjjjjjjrrddddddpffffrrxrffvjjjjrrddddddddjjjjrrjjffjjjjrddddddddrjjjvfpfffjjjjrddddddddddjjjkjrjjjjjjrrrrbdddddddjjjjjkjjjjjjjrrrdddddddddprkjjjjjjjjjrrrddddddddjjjppppjjjjjjrrrrddddddddjjkjjjjjjsssssssbdddddddddbjjjjjjjjjjjddddddddddjrjjjjjkjjrrrrrdddddddddjffkvjjjjkkjrrrrddddddddkjjjkkjjjjjrrrdddddddddjjjjjjjjffkjrrrddddddddddjkjpkkjjjjssjjjjddddddddjjjkrbkjjrrjjjjdddddddddvvvvvbbrjrjjjjdddddddddddjpjjjjjjjjjjjdddddddddjpjfjjjjjjjjjrjjddddddddddjfjjpjjpjjjjrdddddddddpjjjjjjjbvffvpjjddddddddpjjjjjjjjjrrjjdddddddddppbvvvvpjjkfffbjjjrrdddddddkkjjjjjkjjjjdddddddddddpkkjjjjjjfjjjjrddddddddddkjjjjjjjjjjjjdddddddddkjjjjjjjjjjjjjrrddddddddddjjjjpppjjjjjrrdddddddddddkjkjvpvssvbjrrddddddddkfffkjjjjjjjjjrdddddddddkjjjkjjpjjjrrrddddddddkccccccvrjjjjjrddddddpcccccccccbjjjjjjrdddddddccccccccckkjjjjjrddddddbcccccccccjjjjfjjrdddddddcccccccccejjjjjjjddddddddddkpjkjvfffvjjjjddddddddddbpffffjjjjjjjrdddddddcccccccccpjjjjjrddddddddccccccccckrjjjjrdddddddpccccccccckkjjjjjrdddddddccccccccckjjjpfrddddddddcccccccccepkrfkrddddddddddjkkprrjjjjjffffffdddddddddrrkkjjjrjjjjrdddddddcccccccccbkjjjrrddddddddccccccccceekkjjrddddddddcccccccccbjjjkrddddddddddrxjjrkjjppjjrjjddddddddddjjpppjrrrrjjjrrdddddddddrrjjjjrjjfffrrddddddddrrrkkjffjjjjjjjjjdddddddddddjjkjjjjjjjjjrrddddddccccccccceepbkjrrdddddddcccccccccpjjjjjrddddddddccccccccceprrvbfffddddddcccccccccevpffrrddddddddddddbfffffrrrrrkfrrddddddddrjjfffjjjjjjrrddddddpcccccccccevjjjkrddddddddccccccccceecjprrddddddddcccccccccvrjrprdddddddddcccccccccepfbjjrddddddddcccccccccevvrjjjddddddddddjjprjjjjkjjjrdddddddddddjjjkjjjjkjjjjdddddddddcccccccccjjvppvrrdddddddccccccccceevccjjddddddddcccccccccejjcjjrrdddddddcccccccccjjcjjjrbdddddddcccccccccevrjjrrdddddddddddddjjprkjrjjjjddddddddddjjpjjjkjjjjjjrbdddddddcccccccccbjjjjjdddddddddccccccccceejjjjbddddddddcccccccccjjjbjjrddddddddcccccccccbjjjjrrddddddddcccccccccesssvrddddddddddddjkjjjjjjffjjjrddddddddddjjrrjjjjjjjjjrrrddddddddddrjjrjjjjjrrrddddddddddccjjjjjkjjjjjddddddddcccccccccjjjjjjrddddddddcccccccccjjjjjjrrdddddddcccccccccjjjjjjrjdddddddddddjjjjjjcjjjjssssscrdddddddjjjjjjffjjdddddddddcccccccccvjjjjjrddddddddccccccccceejjjjrddddddddcccccccccjjjjjrrddddddddcccccccccpvjjjjjddddddddcccccccccebjjjjrrrdddddddjjjjjjjcjjjjjjrrrddddddddjjjjjjjpjjjjjrrdddddddcccccccccjjjppbrddddddddcccccccccbjjjjjrrdddddddcccccccccvcjjjjjddddddddcccccccccvjjjcjjddddddddccccvccssvrrjjjrrddddddddddrjcjjjjjjjbrrrddddddddddbjjjjjcjjjjjrddddddddcccccccccbjjjjjrrdddddddccccccccceebjjjrddddddddcccccccccejjjjjjddddddddcccccccccvjjrjjrrdddddddddbjjjrcjjjjjjjrdddddddddddjfffjjjcjjjjrrdddddddddddjjjrjcbjjjjrrrddddddcccccccccbrjrrjjrdddddddccccccccceebjjjjddddddddcccccccccbjjjjjjddddddddcccccccccjjprjrdddddddddcccccccccevjjjjdddddddddddfjpjjrppjjjjjjrdddddddddddccfjjjjjjjjrddddddddcccccccccbjfjjjrddddddddccccccccceejfpcrddddddddcccccccccevcrrrrddddddddccccccccvpjjjrrdddddddddcccccccccevfjrbddddddddddddjjfrjjfprrfjsssssssddddddddbrrjjjjjjrrdddddddcccccccccbfpjjddddddddddccccccccceevrfrdddddddddcccccvbrcrprfjjdddddddddcccccccccbdjjrddddddddddcccccccccevjjjjrddddddddddrrjjjjppfffrrrdddddddddddbjfffffjjjrbdddddddddcccccccccbfprjrrddddddddccccccccveebrjrdddddddddddppfjjjcjjbjfrdddddddddcccccssssbrjjjrdddddddddcccccccccebrjjrdddddddddddfcpjjjjjjfjjfrddddddddddbvfffffvjjjrrddddddddddrbjjpkjjjfjjjrrddddddddcccccsssssejjjjrddddddddcccccccccjjjjjjrddddddddcccccccccpvjjjjrddddddddcccccccccpjjjjjrrddddddddddfffjjkkkkvrrrddddddddddddjjjjjfffjjjjbdddddddcccccccccvjjjpjjrdddddddccccccccceevpjrrddddddddcccccccccjjjjjjjddddddddcccccccccccpfcrrddddddddccccccccceebrrccrddddddddddddcccffjjjjjrrdddddddddjrjpjjjbffffjjjrdddddddddrjjjjpjjjjjjrddddddddcccccccccbjjcrrrddddddddcccccccccpprrrjjddddddddcccccccccvjrjjjrddddddddcccccccccebjjjjrrrrddddddjjjjjjppjjrjjjjddddddddddddjffpjjfffjjjrdddddddddddpvvvvvppjjjjddddddddddbjjffjjjpjjjjjjddddddddddcccrrfpjjjjjjrdddddddddddbpccjjccjjjjbdddddddddddpcjjjcvffbpjddddddddddddcjjjcccjbfjjrrddddddddddjjjjjjjjjjjfrdddddddddddcxfjjccjjjjjrdddddddddddvvvvvppjjjkkrrddddddddddbjjjppppjjjrrddddddddddjjpjjjjjjpjjrrddddddddddppjjjjjjjjsssssssdddddddddrjjfkkkfjjjrrdddddddddddfffffjjjjjrrrddddddcccccccccfjjjjjjrrddddddcccccccccjjjjjjjrdddddddcccccccccjjjjpjjddddddddcccccccccvppjjjrddddddddcccccccccebjjjjrddddddddddddjjjjrrjjjjjrrddddddddddrjjvfffffbjjjddddddddcccccccccbjjjjjjddddddddccccccccceebpjjjddddddddcccccccccebjjjjjrrddddddddddpkrjjjjjkjjrddddddddcccccccccejjjjjjffdddddddddkjjprjjjcrjjjjrddddddddpppkjjjjfffpcrrdddddddcccccccccvjrffjrddddddddcccccccccebjjjjjddddddddcccccccccjjjjjcrddddddddcccccccccbjjjjrrddddddddcccccccccejpffffdddddddddddjjppvjvbbbvvvvvvvvvvvvvvvvvvvvvvvvvvbsbvvvvvvvvvvvvvvvvvvbbbbbppppbxxvvrprrdddddpprrxrrpppvbddrddddddbrddpvrrxrssrvbrprrrddddddddprvrrrrvssspvrbddddddddddrrvssvrxxpbrddddddxbvrrsrvbssvrrpbrrrdddddddddddddpbvvvpprrrrdddddddddbrssssscvssssrrdddddddddbssrpbppcssssrrdddddddddbbcrrppppbssvrrdddddddddrbspxpppbsssssbvdddddddrrbbpspsrrxsssrrdddddddddddprvvrrxssvxbrrdddddddpvbrsrbvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvdddddddrrrrrffffkbrppjjjrdddddddcccccccccvbrpjjjrdddddddcccccccccepfrddddddddddddddbvfffrjrddddddddddddddddrprrbrrkrrjjrddddddddcccccccccbrpjjjddddddddddjjppcbjjjjjjkrdrdddddddcccccccccerppkjjddddddddcccccccccjkpjjjbddddddddddrrkkrppjjjjxrrrddddddddddrprkrjjjxjjjrbddddddddddbpjjjjjjjxjrrrdddddddcccccccccbrjjjjrbdddddddccccccccceejjjxrddddddddcccccccccerrjjjdddddddddcccccccccerjjjrddddddddddjrvvsssssvrjjjrrddddddddddrrrrrjjjjjrrrdddddddddddrrrjjjffffrrrddddddddcccccccccbjjjjjjddddddddddbrjjjjjjxrrrrrddddddddcccccccccjjjjjjjddddddddcccccccccjjcjxjjrdddddddcccccccccebjcjjjjjdddddddddbjffffjjjpjjjrddddddddddpppppjjjkjrcbbdddddddcccccccccbjcjjcjrrddddddcccccccccxjjrjjjddddddddcccccccccjjjjprjrdddddddcccccccccrjjcjjrbdddddddcccccccccerpjjrjdddddddddddrpjjjccrrjjrrddddddddddpppjjjcscsjxjjrdddddddccccccccccccjjjjddddddddcccccccccbjrrrjjddddddddcccccccccjjjcrjjddddddddcccccccccbsrssssddddddddcccccccccssjsrsrrddddddddbrbsssssssssssssssbdddddrjpcrjjccrcbssdrddddddddrjjccccpbrxjjrdrdddddddcccccccccrpjjjcdrdddddddcccccccccrjxkjjdbdddddddcccccccccbjjjrbrdddddddddfjjjrrrrjjpjjjbssssbddddddddjjrrpfpcjjrdddddddddddrrjffffjjrpcrddddddddcccccccccvjjspjrddddddddcccccccccvbcjjjrddddddddcccccccccjjjcjjrddddddddcccccccccrjjbpjrddddddddcccccccccebjjjjjdddddddddddbjjprjjjcrjjjddddddddddpppbjjjjfffpcrddddddddcccccccccspsssjjddddddddcccccccccerjjcjjddddddddcccccccccbjjffffddddddddceeccceecrjjbpjrddddddddcceeeeeecebjjjjjddddddddddccccccrrjjpjjjssssbdddddddjjjjjjjjjjcjddddddddcccccccccspsssjjddddddddcccccccccerjjcjjddddddddcccccccccbjjjjjjddddddddcccccccccrjjbpjrddddddddcccccccccebjjjjjdddddddddddpjfffvcjjrrrrddddddddddjjjcpjjjjjjjrdddddddddccccccccjjjcjjrrddddddddcccccccccerjjjrdddddddddcccccccccjjjjjjjddddddddcccccccccjjcjxjjddddddddcccccccccessppjpdddddbdddpjsssssvjjrjvvssssssssvddddddddjjjvssvdddddddddddjjsssssrjjjrjjddddddddcccccccccerjjjrdddddddddcccccccccjjjjjjjddddddddcccccccccjjcjxjjddddddddcccccccccessppjpdddddddddddjjjjjjsjjjjjjjdddddddddjjrrrrrjjpjbjjrjddddddddjjrrrrrjjjjjjjdddddddddddpjjjjvebjjbjjrrrdddddddbjjjjrrjjjjjjjjddddddddddpjjjjjjrjjjjjbddddddddddxjjjjjjjjjjjjddddddddddjjjjrrrjjjpjjjjrbddddddddjjjjjjjjjbjjjjbdddddddddjjrjjveejjjjjjdddddddddddjpfjbeejjjjjrddddddddddjjjjjeeebfjjjddddddddbjjjkjpjbeessvjrrddddddddddjjjkjpjjeesssssbdddddddddddjjjfffrrjjrdddddddddddjjjjjjjjpjjjrdddddddddddpjjjjrrrjjjjdddddddddddjjrjjveejjjjjddddddddddddjpfjbeejjjjjjdddddddeeeeefvjkrjjjjjjjjrdddddeeeebssvkrjjjjrrjrdddddddddjjjjjjjjjjjjjjrrddddddddjjjrjjjjjjjjjjjrdddddddjjjjjjjeejjjjjjrdddddddddjffffveejjjfffddddddddddjjjjjveejjjjjjjddddddddddjjjjveejjjjjjrddddddddddjjjjveejjjjjjrrddddddddrjjjbrveepeeejjrrdddddddjjjrrrrfffjjrjjrdddddddddbjjpfffffffffrrdddddddjjjjrjjjjjfjjrjrdddddddddjjjjjjrjjfjjjjrrddddddddddjjjrrjjjfjjrbddddddddddvvvvvppbrffffrrrbddddddbrjjjjjjjjjjjjjrddddddddddjjjjjjsssssvprdddddddddbjprfssssjjj";
  var colors = {
    d: 'hsl(225, 45%, 28%)',
    j: 'hsl(25, 85%, 58%)',
    c: 'hsl(35, 25%, 58%)',
    r: 'hsl(195, 60%, 78%)',
    s: 'hsl(340, 65%, 68%)',
    p: 'hsl(150, 25%, 48%)',
    f: 'hsl(48, 75%, 62%)',
    v: 'hsl(5, 70%, 52%)',
    b: 'hsl(0, 0%, 80%)',
    e: 'hsl(175, 40%, 42%)',
    k: 'hsl(280, 40%, 62%)',
    x: 'hsl(100, 50%, 46%)'
  };
  var canvas = document.getElementById('vida-grid');
  var ctx = canvas.getContext('2d');
  var cw = 14, ch = 6;
  for (var day = 0; day < 365; day++) {
    for (var hour = 0; hour < 24; hour++) {
      ctx.fillStyle = colors[data[day * 24 + hour]];
      ctx.fillRect(hour * cw, day * ch, cw, ch);
    }
  }
  var monthDays = [31,28,31,30,31,30,31,31,30,31,30,31];
  var y = 0;
  ctx.fillStyle = 'rgba(238, 224, 203, 0.6)';
  for (var m = 0; m < 11; m++) {
    y += monthDays[m] * ch;
    ctx.fillRect(0, y - 1, canvas.width, 1);
  }
})();
</script>
