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
.vida-photo-tooltip {
  position: fixed;
  display: none;
  z-index: 1000;
  width: 150px;
  background: #fff8ef;
  border: 1px solid #BAA898;
  border-radius: 6px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.18);
  padding: 6px;
  pointer-events: none;
  font-size: 0.7rem;
  line-height: 1.35;
}
.vida-photo-tooltip img {
  display: block;
  width: 100%;
  height: 138px;
  object-fit: cover;
  border-radius: 4px;
  margin-bottom: 4px;
}
.vida-photo-tooltip .vida-photo-place {
  font-weight: bold;
  color: #4a5a4f;
}
.vida-photo-tooltip .vida-photo-date {
  color: #999;
}
.vida-photo-tooltip .vida-photo-cat {
  display: flex;
  align-items: center;
  gap: 0.3rem;
  margin-top: 2px;
}
.vida-photo-tooltip .vida-photo-cat .vida-swatch {
  width: 9px;
  height: 9px;
}
.vida-hint {
  text-align: center;
  font-size: 0.75rem;
  color: #999;
  margin: -0.6rem 0 0.6rem 0;
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

<p class="vida-hint">A few tiles have a white border. Hover (or tap) one to see the photo.</p>

<div class="vida-photo-tooltip" id="vida-photo-tooltip">
  <img src="" alt="">
  <div class="vida-photo-place"></div>
  <div class="vida-photo-date"></div>
  <div class="vida-photo-cat"><span class="vida-swatch"></span><span></span></div>
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
    <div class="vida-comment" style="top:54%"><b>Mid July.</b> Winter break in Argentina, so a week at the beach in Villa Gesell. The two five hour travel streaks on either side, July 16 and July 24, are the road trip there and back.</div>
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

  var catNames = {
    d: 'Sleep', j: 'Games', c: 'School', r: 'Relax', s: 'Social', p: 'Productive',
    f: 'Family', v: 'Travel', b: 'Junk', e: 'Extracurricular', k: 'Creative', x: 'Exercise'
  };
  var photos = [
    {day: 42,  hour: 15, src: '/2018_pictures/thumbs/IMG_20180212_154237.jpg', date: 'Feb 12, 2018', time: '3:42pm', place: 'Buenos Aires', cat: 'f'},
    {day: 151, hour: 11, src: '/2018_pictures/thumbs/IMG_20180601_111145.jpg', date: 'Jun 1, 2018', time: '11:11am', place: 'Buenos Aires', cat: 'c'},
    {day: 163, hour: 11, src: '/2018_pictures/thumbs/IMG_20180613_112234.jpg', date: 'Jun 13, 2018', time: '11:22am', place: 'Buenos Aires', cat: 'c'},
    {day: 185, hour: 15, src: '/2018_pictures/thumbs/IMG_20180705_153658.jpg', date: 'Jul 5, 2018', time: '3:36pm', place: 'Buenos Aires', cat: 'c'},
    {day: 203, hour: 13, src: '/2018_pictures/thumbs/IMG_20180723_134159.jpg', date: 'Jul 23, 2018', time: '1:42pm', place: 'Villa Gesell', cat: 'x'},
    {day: 208, hour: 18, src: '/2018_pictures/thumbs/IMG_20180728_183053.jpg', date: 'Jul 28, 2018', time: '6:30pm', place: 'Buenos Aires', cat: 'k'},
    {day: 230, hour: 1,  src: '/2018_pictures/thumbs/IMG_20180819_010357.jpg', date: 'Aug 19, 2018', time: '1:03am', place: 'Rio de Janeiro', cat: 'v'},
    {day: 231, hour: 12, src: '/2018_pictures/thumbs/IMG_20180821_004023.jpg', date: 'Aug 21, 2018', time: '12:40am', place: 'Tokyo', cat: 'v'},
    {day: 231, hour: 21, src: '/2018_pictures/thumbs/IMG_20180821_095826.jpg', date: 'Aug 21, 2018', time: '9:58am', place: 'Tokyo', cat: 'b'},
    {day: 233, hour: 5,  src: '/2018_pictures/thumbs/IMG_20180822_171557_01.jpg', date: 'Aug 22, 2018', time: '5:15pm', place: 'Tokyo', cat: 'v'},
    {day: 239, hour: 7,  src: '/2018_pictures/thumbs/IMG_20180828_190909.jpg', date: 'Aug 28, 2018', time: '7:09pm', place: 'Morioka', cat: 's'},
    {day: 243, hour: 0,  src: '/2018_pictures/thumbs/IMG_20180901_125116.jpg', date: 'Sep 1, 2018', time: '12:51pm', place: 'Morioka', cat: 'p'},
    {day: 244, hour: 22, src: '/2018_pictures/thumbs/IMG_20180903_101841.jpg', date: 'Sep 3, 2018', time: '10:18am', place: 'Tochigi', cat: 's'},
    {day: 245, hour: 4,  src: '/2018_pictures/thumbs/IMG_20180903_161247.jpg', date: 'Sep 3, 2018', time: '4:12pm', place: 'Tokyo', cat: 'v'},
    {day: 284, hour: 17, src: '/2018_pictures/thumbs/IMG_20181012_175253.jpg', date: 'Oct 12, 2018', time: '5:52pm', place: null, cat: 's'},
    {day: 314, hour: 20, src: '/2018_pictures/thumbs/IMG_20181111_201621.jpg', date: 'Nov 11, 2018', time: '8:16pm', place: null, cat: 'j'}
  ];
  var photoMap = {};
  photos.forEach(function (p) {
    photoMap[p.day * 24 + p.hour] = p;
  });

  ctx.lineWidth = 1;
  ctx.strokeStyle = '#fff';
  photos.forEach(function (p) {
    ctx.strokeRect(p.hour * cw + 0.5, p.day * ch + 0.5, cw - 1, ch - 1);
  });

  var tooltip = document.getElementById('vida-photo-tooltip');
  var tooltipImg = tooltip.querySelector('img');
  var tooltipPlace = tooltip.querySelector('.vida-photo-place');
  var tooltipDate = tooltip.querySelector('.vida-photo-date');
  var tooltipCatSwatch = tooltip.querySelector('.vida-photo-cat .vida-swatch');
  var tooltipCatName = tooltip.querySelector('.vida-photo-cat span:last-child');

  function cellAt(clientX, clientY) {
    var rect = canvas.getBoundingClientRect();
    var x = (clientX - rect.left) * (canvas.width / rect.width);
    var yy = (clientY - rect.top) * (canvas.height / rect.height);
    var hour = Math.floor(x / cw);
    var day = Math.floor(yy / ch);
    if (hour < 0 || hour > 23 || day < 0 || day > 364) return null;
    return photoMap[day * 24 + hour] || null;
  }

  function showTooltip(photo, clientX, clientY) {
    tooltipImg.src = photo.src;
    if (photo.place) {
      tooltipPlace.textContent = photo.place;
      tooltipDate.textContent = photo.date + ' · ' + photo.time;
    } else {
      tooltipPlace.textContent = photo.date;
      tooltipDate.textContent = photo.time;
    }
    tooltipCatSwatch.style.background = colors[photo.cat];
    tooltipCatName.textContent = catNames[photo.cat];
    tooltip.style.display = 'block';
    var tw = 164, th = 205;
    var left = clientX + 14;
    var top = clientY + 14;
    if (left + tw > window.innerWidth) left = clientX - 14 - tw;
    if (top + th > window.innerHeight) top = clientY - 14 - th;
    tooltip.style.left = left + 'px';
    tooltip.style.top = top + 'px';
  }

  function hideTooltip() {
    tooltip.style.display = 'none';
    canvas.style.cursor = 'default';
  }

  canvas.addEventListener('mousemove', function (e) {
    var photo = cellAt(e.clientX, e.clientY);
    if (photo) {
      canvas.style.cursor = 'pointer';
      showTooltip(photo, e.clientX, e.clientY);
    } else {
      hideTooltip();
    }
  });
  canvas.addEventListener('mouseleave', hideTooltip);

  canvas.addEventListener('touchstart', function (e) {
    var t = e.touches[0];
    var photo = cellAt(t.clientX, t.clientY);
    if (photo) {
      e.preventDefault();
      if (tooltip.style.display === 'block' && tooltipImg.src.indexOf(photo.src) !== -1) {
        hideTooltip();
      } else {
        showTooltip(photo, t.clientX, t.clientY);
      }
    } else {
      hideTooltip();
    }
  }, { passive: false });

  document.addEventListener('touchstart', function (e) {
    if (e.target !== canvas) {
      hideTooltip();
    }
  });
})();
</script>
