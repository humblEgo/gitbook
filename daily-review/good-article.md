# Good article 아카이빙

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xD0A4;&#xC6CC;&#xB4DC;</th>
      <th style="text-align:left">&#xB9C1;&#xD06C;</th>
      <th style="text-align:left">&#xC694;&#xC57D;</th>
      <th style="text-align:left">&#xC219;&#xC9C0;&#xC815;&#xB3C4;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">DB</td>
      <td style="text-align:left"><a href="https://transferhwang.tistory.com/513">[MySQL] &#xACA9;&#xB9AC; &#xC218;&#xC900;</a>
      </td>
      <td style="text-align:left">&#xB370;&#xC774;&#xD130; &#xBD80;&#xC815;&#xD569; &#xBB38;&#xC81C;&#xB97C;
        &#xC608;&#xC2DC;&#xC640; &#xD568;&#xAED8; &#xD655;&#xC778; &#xAC00;&#xB2A5;</td>
      <td
      style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">DB</td>
      <td style="text-align:left"><a href="https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/">Lock&#xC73C;&#xB85C; &#xC774;&#xD574;&#xD558;&#xB294; Transaction&#xC758; Isolation Level</a>
      </td>
      <td style="text-align:left">
        <p>MySQL&#xC758; &#xB3D9;&#xC791;&#xC6D0;&#xB9AC;&#xAE4C;&#xC9C0; &#xC605;&#xBCFC;
          &#xC218; &#xC788;&#xB294; &#xC88B;&#xC740; &#xAE00;!</p>
        <ul>
          <li>InnoDB &#xC5D4;&#xC9C4;&#xC740; &#xAC01; &#xCFFC;&#xB9AC;&#xB97C; &#xC2E4;&#xD589;&#xD560;
            &#xB54C;&#xB9C8;&#xB2E4; &#xC2E4;&#xD589;&#xD55C; &#xCFFC;&#xB9AC;&#xC758;
            log&#xB97C; &#xCC28;&#xACE1;&#xCC28;&#xACE1; &#xC800;&#xC7A5;&#xD55C;&#xB2E4;.
            &#xADF8;&#xB9AC;&#xACE0; &#xB098;&#xC911;&#xC5D0; consistent read&#xB97C;
            &#xD560; &#xB54C; &#xC774; log&#xB97C; &#xD1B5;&#xD574; &#xD2B9;&#xC815;
            &#xC2DC;&#xC810;&#xC758; DB snapshot&#xC744; &#xBCF5;&#xAD6C;&#xD558;&#xC5EC;
            &#xAC00;&#xC838;&#xC628;&#xB2E4;.</li>
        </ul>
      </td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">DB</td>
      <td style="text-align:left"><a href="https://jupiny.com/2018/11/30/mysql-transaction-isolation-levels/">MySQL&#xC758; Transaction Isolation Levels</a>
      </td>
      <td style="text-align:left">
        <ul>
          <li>MySQL&#xC5D0;&#xC11C;&#xB294; <code>REPEATABLE READ</code>&#xC640; <code>READ COMMITTED</code> &#xB808;&#xBCA8;
            &#xACA9;&#xB9AC; &#xC218;&#xC900;&#xC5D0;&#xC11C; <code>SELECT</code> &#xCFFC;&#xB9AC;&#xB85C;
            &#xB370;&#xC774;&#xD130;&#xB97C; &#xC77D;&#xC5B4;&#xC62C; &#xB54C;, &#xD14C;&#xC774;&#xBE14;&#xC5D0;
            lock&#xC744; &#xAC78;&#xC9C0; &#xC54A;&#xACE0;, &#xD574;&#xB2F9; &#xC2DC;&#xC810;&#xC758;
            &#xB370;&#xC774;&#xD130; &#xC0C1;&#xD0DC;&#xB97C; &#xC758;&#xBBF8;&#xD558;&#xB294;
            snapshot&#xC744; &#xAD6C;&#xCD95;&#xD558;&#xC5EC; &#xB370;&#xC774;&#xD130;&#xB97C;
            &#xAC00;&#xC838;&#xC628;&#xB2E4;.
            <ul>
              <li>&#xB54C;&#xBB38;&#xC5D0; <code>REPEATABLE READ</code>&#xC5D0;&#xC11C; <code>phantom read</code>&#xB3C4;
                &#xBC1C;&#xC0DD;&#xD558;&#xC9C0; &#xC54A;&#xB294;&#xB2E4;.</li>
            </ul>
          </li>
        </ul>
      </td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">architecture</td>
      <td style="text-align:left"><a href="https://gyrfalcon.tistory.com/entry/%EB%9E%8C%EB%8B%A4-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-Lambda-Architecture">&#xB78C;&#xB2E4; &#xC544;&#xD0A4;&#xD14D;&#xCC98;(Lambda Architecture)</a>
      </td>
      <td style="text-align:left">
        <ul>
          <li>&#xC2E4;&#xC2DC;&#xAC04; &#xBD84;&#xC11D;&#xC744; &#xC9C0;&#xC6D0;&#xD558;&#xB294;
            &#xBE45;&#xB370;&#xC774;&#xD130; &#xC544;&#xD0A4;&#xD14D;&#xCC98;</li>
          <li>batch&#xB85C; &#xBBF8;&#xB9AC; &#xB9CC;&#xB4E0; &#xB370;&#xC774;&#xD130;&#xC640;
            &#xC2E4;&#xC2DC;&#xAC04; &#xB370;&#xC774;&#xD130;&#xB97C; &#xD63C;&#xD569;&#xD574;&#xC11C;
            &#xC0AC;&#xC6A9;&#xD558;&#xB294; &#xBC29;&#xC2DD;</li>
        </ul>
      </td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">cloud</td>
      <td style="text-align:left"><a href="https://medium.com/dtevangelist/12-factors-%EB%9E%80-b39c7ef1ed30">12-Factors&#xB780;?</a>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">test</td>
      <td style="text-align:left"><a href="https://www.reimaginer.me/entry/%ED%85%8C%EC%8A%A4%ED%8A%B8%EC%97%90-%EB%8C%80%ED%95%9C-%EC%8B%A4%EC%9A%A9%EC%A0%81%EC%9D%B8-%EC%A0%91%EA%B7%BC-Humble-Object-Pattern">&#xD14C;&#xC2A4;&#xD2B8;&#xC5D0; &#xB300;&#xD55C; &#xC2E4;&#xC6A9;&#xC801;&#xC778; &#xC811;&#xADFC;</a>
      </td>
      <td style="text-align:left">&#xD750;&#xB984;&#xC81C;&#xC5B4; &#xCF54;&#xB4DC;&#xC640; &#xC2E4;&#xC81C;
        &#xBE44;&#xC988;&#xB2C8;&#xC2A4; &#xB85C;&#xC9C1;&#xC744; &#xBD84;&#xB9AC;&#xD574;&#xC11C;
        &#xD14C;&#xC2A4;&#xD2B8;&#xB97C; &#xC791;&#xC131;&#xD558;&#xC790;.</td>
      <td
      style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">architecture</td>
      <td style="text-align:left"><a href="https://geminikim.medium.com/%EC%A7%80%EC%86%8D-%EC%84%B1%EC%9E%A5-%EA%B0%80%EB%8A%A5%ED%95%9C-%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4%EB%A5%BC-%EB%A7%8C%EB%93%A4%EC%96%B4%EA%B0%80%EB%8A%94-%EB%B0%A9%EB%B2%95-97844c5dab63">&#xC9C0;&#xC18D;&#xC131;&#xC7A5; &#xAC00;&#xB2A5;&#xD55C; &#xC18C;&#xD504;&#xD2B8;&#xC6E8;&#xC5B4;&#xB97C; &#xB9CC;&#xB4DC;&#xB294; &#xBC29;&#xBC95;</a>
      </td>
      <td style="text-align:left">
        <p>Business Logic</p>
        <p>&#xC0C1;&#xC138; &#xAD6C;&#xD604; &#xB85C;&#xC9C1;&#xC740; &#xC798; &#xBAA8;&#xB974;&#xB354;&#xB77C;&#xB3C4;
          &#xBE44;&#xC988;&#xB2C8;&#xC2A4;&#xC758; &#xD750;&#xB984;&#xC740; &#xC774;&#xD574;
          &#xAC00;&#xB2A5;&#xD55C; &#xB85C;&#xC9C1;&#xC774;&#xC5B4;&#xC57C; &#xD55C;&#xB2E4;.</p>
        <p></p>
        <p>Layer</p>
        <p>&#xB808;&#xC774;&#xC5B4; &#xAC04;,&#xB808;&#xC774;&#xC5B4; &#xB0B4; &#xCC38;&#xC870;&#xADDC;&#xCE59;&#xB4E4;
          &#xD655;&#xC778;&#xD560; &#xAC83;.</p>
        <p></p>
        <p>Module</p>
        <p>&#xC7AC;&#xC0AC;&#xC6A9;&#xC131;&#xC744; &#xACE0;&#xB824;&#xD558;&#xC5EC;
          &#xC124;&#xACC4;&#xD558;&#xC790;.</p>
      </td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">collaboration</td>
      <td style="text-align:left"><a href="https://pitzcarraldo.medium.com/%EB%B2%88%EC%97%AD-%EC%9E%98-%EA%B0%80%EC%9A%94-%EC%8A%A4%ED%81%AC%EB%9F%BC-%EB%B0%98%EA%B0%80%EC%9B%8C%EC%9A%94-%EC%B9%B8%EB%B0%98-e27d1db15699">&#xC798; &#xAC00;&#xC694; &#xC2A4;&#xD06C;&#xB7FC;,&#xBC18;&#xAC00;&#xC6CC;&#xC694; &#xCE78;&#xBC18;</a>
      </td>
      <td style="text-align:left">&#xC2A4;&#xD50C;&#xB9B0;&#xD2B8; &amp;&#xC2A4;&#xD06C;&#xB7FC;&#xC758;
        &#xB2E8;&#xC810;&#xC774; &#xB290;&#xAEF4;&#xC9C4;&#xB2E4;&#xBA74; &#xCE78;&#xBC18;&#xC744;
        &#xACE0;&#xB824;&#xD558;&#xC790;.</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">architecture</td>
      <td style="text-align:left"><a href="https://www.slideshare.net/Byungwook/4-61487454">&#xB300;&#xC6A9;&#xB7C9; &#xC544;&#xD0A4;&#xD14D;&#xD2B8; &#xC124;&#xACC4; &#xD328;&#xD134;</a>
      </td>
      <td style="text-align:left">
        <p>&#xC544;&#xB798; &#xD328;&#xD134;&#xB4E4;&#xC774; &#xC788;&#xB2E4;.
          <br
          />&#xC11C;&#xBE44;&#xC2A4; &#xC9C0;&#xD5A5;&#xC801;</p>
        <p>Redudant &amp; Resilience</p>
        <p>&#xD30C;&#xD2F0;&#xC154;&#xB2DD;</p>
        <p>Query Off Loading</p>
        <p>&#xCE90;&#xC2F1;</p>
        <p>CDN &amp; ADN</p>
        <p>&#xB85C;&#xAE45;</p>
        <p>&#xBE44;&#xB3D9;&#xAE30; &#xD328;&#xD134;</p>
      </td>
      <td style="text-align:left">1</td>
    </tr>
  </tbody>
</table>



