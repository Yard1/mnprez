<style>
.reveal h1, .reveal h2, .reveal h3 {
  word-wrap: normal;
  -moz-hyphens: none;
}
.small-code pre code {
  font-size: 1em;
}
.midcenter {
    position: fixed;
    top: 50%;
    left: 50%;
}
.footer {
    color: black; background: #E8E8E8;
    position: fixed; top: 90%;
    text-align:center; width:100%;
}
.pinky .reveal .state-background {
  background: #FF69B4;
} 
.pinky .reveal h1,
.pinky .reveal h2,
.pinky .reveal p {
  color: black;
}
</style>



Projekt Poprawkowy
========================================================
author: "Antoni Baum, Bartłomiej Gąsior, Paweł Sumara"
date: 
autosize: true

Wprowadzenie
========================================================

Celem projektu jest budowa modelu liniowego dla spalania w podanych danych. Aby otrzymać model, użyte zostaną zarówno klasyczne metody ekonometryczne, jak i metody bootstrapowe.

Zmienne
========================================================
Zmienna objaśniana

- **l100km** - spalanie w litrach na 100 kilometrów.

Zmienne objaśniające

- **cylinders** - liczba cylindrów,
- **displacement** - objętość silnika w litrach,
- **horsepower** - moc w koniach mechanicznych,
- **weight** - waga w kilogramach,
- **acceleration** - czas przyspieszenia od 0 do 100 kilometrów na godzinę, podany w sekundach,
- **year** - rok produkcji,
- **origin** - miejsce produkcji (1 - USA, 2 - Europa, 3 - Japonia),
- **name** - nazwa samochodu.

Format danych
========================================================

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> l100km </th>
   <th style="text-align:right;"> cylinders </th>
   <th style="text-align:right;"> displacement </th>
   <th style="text-align:right;"> horsepower </th>
   <th style="text-align:right;"> weight </th>
   <th style="text-align:right;"> acceleration </th>
   <th style="text-align:right;"> year </th>
   <th style="text-align:right;"> origin </th>
   <th style="text-align:left;"> name </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 9.800608 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.474836 </td>
   <td style="text-align:right;"> 75 </td>
   <td style="text-align:right;"> 956.1727 </td>
   <td style="text-align:right;"> 16.275 </td>
   <td style="text-align:right;"> 74 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> fiat 128 </td>
  </tr>
</tbody>
</table>

Hipotezy
========================================================
Na samym początku można załozyć, że nazwa samochodu nie będzie miała wpływu na spalanie. Najprawdopodobniej, rok produkcji (im nowszy samochód, tym mniejsze spalanie) oraz waga (im cięższy samochód, tym większe spalanie) będą miały największy wpływ.

Z tego też powodu, odrzucono zmienną **name**.



Statystyki opisowe
========================================================
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> Mean </th>
   <th style="text-align:right;"> Median </th>
   <th style="text-align:right;"> Min. </th>
   <th style="text-align:right;"> Max. </th>
   <th style="text-align:right;"> Std. dev. </th>
   <th style="text-align:right;"> CV </th>
   <th style="text-align:right;"> Skewness </th>
   <th style="text-align:right;"> Kurtosis </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> l100km </td>
   <td style="text-align:right;"> 11.219371 </td>
   <td style="text-align:right;"> 10.226722 </td>
   <td style="text-align:right;"> 5.047523 </td>
   <td style="text-align:right;"> 26.134955 </td>
   <td style="text-align:right;"> 3.9091189 </td>
   <td style="text-align:right;"> 0.3484259 </td>
   <td style="text-align:right;"> 0.7546959 </td>
   <td style="text-align:right;"> 0.0769452 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> cylinders </td>
   <td style="text-align:right;"> 5.490446 </td>
   <td style="text-align:right;"> 4.000000 </td>
   <td style="text-align:right;"> 3.000000 </td>
   <td style="text-align:right;"> 8.000000 </td>
   <td style="text-align:right;"> 1.7111468 </td>
   <td style="text-align:right;"> 0.3116590 </td>
   <td style="text-align:right;"> 0.4895637 </td>
   <td style="text-align:right;"> -1.4181049 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> displacement </td>
   <td style="text-align:right;"> 3.182978 </td>
   <td style="text-align:right;"> 2.474447 </td>
   <td style="text-align:right;"> 1.114320 </td>
   <td style="text-align:right;"> 7.456114 </td>
   <td style="text-align:right;"> 1.7205287 </td>
   <td style="text-align:right;"> 0.5405405 </td>
   <td style="text-align:right;"> 0.6699253 </td>
   <td style="text-align:right;"> -0.8503270 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> horsepower </td>
   <td style="text-align:right;"> 104.598726 </td>
   <td style="text-align:right;"> 95.000000 </td>
   <td style="text-align:right;"> 46.000000 </td>
   <td style="text-align:right;"> 230.000000 </td>
   <td style="text-align:right;"> 38.1476633 </td>
   <td style="text-align:right;"> 0.3647049 </td>
   <td style="text-align:right;"> 1.0228742 </td>
   <td style="text-align:right;"> 0.5131599 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> weight </td>
   <td style="text-align:right;"> 1349.139721 </td>
   <td style="text-align:right;"> 1270.512228 </td>
   <td style="text-align:right;"> 731.644493 </td>
   <td style="text-align:right;"> 2331.464782 </td>
   <td style="text-align:right;"> 389.8955852 </td>
   <td style="text-align:right;"> 0.2889957 </td>
   <td style="text-align:right;"> 0.4744613 </td>
   <td style="text-align:right;"> -0.9000924 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> acceleration </td>
   <td style="text-align:right;"> 16.263296 </td>
   <td style="text-align:right;"> 16.275000 </td>
   <td style="text-align:right;"> 8.400000 </td>
   <td style="text-align:right;"> 24.885000 </td>
   <td style="text-align:right;"> 2.8714069 </td>
   <td style="text-align:right;"> 0.1765575 </td>
   <td style="text-align:right;"> 0.1791872 </td>
   <td style="text-align:right;"> 0.1994252 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> year </td>
   <td style="text-align:right;"> 75.984076 </td>
   <td style="text-align:right;"> 76.000000 </td>
   <td style="text-align:right;"> 70.000000 </td>
   <td style="text-align:right;"> 82.000000 </td>
   <td style="text-align:right;"> 3.6276011 </td>
   <td style="text-align:right;"> 0.0477416 </td>
   <td style="text-align:right;"> -0.0084619 </td>
   <td style="text-align:right;"> -1.1043831 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> origin </td>
   <td style="text-align:right;"> 1.608280 </td>
   <td style="text-align:right;"> 1.000000 </td>
   <td style="text-align:right;"> 1.000000 </td>
   <td style="text-align:right;"> 3.000000 </td>
   <td style="text-align:right;"> 0.8206957 </td>
   <td style="text-align:right;"> 0.5102940 </td>
   <td style="text-align:right;"> 0.8314067 </td>
   <td style="text-align:right;"> -1.0038056 </td>
  </tr>
</tbody>
</table>

Macierz korelacji
========================================================
<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> l100km </th>
   <th style="text-align:right;"> cylinders </th>
   <th style="text-align:right;"> displacement </th>
   <th style="text-align:right;"> horsepower </th>
   <th style="text-align:right;"> weight </th>
   <th style="text-align:right;"> acceleration </th>
   <th style="text-align:right;"> year </th>
   <th style="text-align:right;"> origin </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> l100km </td>
   <td style="text-align:right;"> 1.0000000 </td>
   <td style="text-align:right;"> 0.8391160 </td>
   <td style="text-align:right;"> 0.8684797 </td>
   <td style="text-align:right;"> 0.8541834 </td>
   <td style="text-align:right;"> 0.8947414 </td>
   <td style="text-align:right;"> -0.4201332 </td>
   <td style="text-align:right;"> -0.5526546 </td>
   <td style="text-align:right;"> -0.5514695 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> cylinders </td>
   <td style="text-align:right;"> 0.8391160 </td>
   <td style="text-align:right;"> 1.0000000 </td>
   <td style="text-align:right;"> 0.9507553 </td>
   <td style="text-align:right;"> 0.8454506 </td>
   <td style="text-align:right;"> 0.8991250 </td>
   <td style="text-align:right;"> -0.4776420 </td>
   <td style="text-align:right;"> -0.3518177 </td>
   <td style="text-align:right;"> -0.5862175 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> displacement </td>
   <td style="text-align:right;"> 0.8684797 </td>
   <td style="text-align:right;"> 0.9507553 </td>
   <td style="text-align:right;"> 1.0000000 </td>
   <td style="text-align:right;"> 0.8959796 </td>
   <td style="text-align:right;"> 0.9366893 </td>
   <td style="text-align:right;"> -0.5242503 </td>
   <td style="text-align:right;"> -0.3747302 </td>
   <td style="text-align:right;"> -0.6335954 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> horsepower </td>
   <td style="text-align:right;"> 0.8541834 </td>
   <td style="text-align:right;"> 0.8454506 </td>
   <td style="text-align:right;"> 0.8959796 </td>
   <td style="text-align:right;"> 1.0000000 </td>
   <td style="text-align:right;"> 0.8697402 </td>
   <td style="text-align:right;"> -0.6761240 </td>
   <td style="text-align:right;"> -0.4232779 </td>
   <td style="text-align:right;"> -0.4747649 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> weight </td>
   <td style="text-align:right;"> 0.8947414 </td>
   <td style="text-align:right;"> 0.8991250 </td>
   <td style="text-align:right;"> 0.9366893 </td>
   <td style="text-align:right;"> 0.8697402 </td>
   <td style="text-align:right;"> 1.0000000 </td>
   <td style="text-align:right;"> -0.3867368 </td>
   <td style="text-align:right;"> -0.3186360 </td>
   <td style="text-align:right;"> -0.6039929 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> acceleration </td>
   <td style="text-align:right;"> -0.4201332 </td>
   <td style="text-align:right;"> -0.4776420 </td>
   <td style="text-align:right;"> -0.5242503 </td>
   <td style="text-align:right;"> -0.6761240 </td>
   <td style="text-align:right;"> -0.3867368 </td>
   <td style="text-align:right;"> 1.0000000 </td>
   <td style="text-align:right;"> 0.2815230 </td>
   <td style="text-align:right;"> 0.2275221 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> year </td>
   <td style="text-align:right;"> -0.5526546 </td>
   <td style="text-align:right;"> -0.3518177 </td>
   <td style="text-align:right;"> -0.3747302 </td>
   <td style="text-align:right;"> -0.4232779 </td>
   <td style="text-align:right;"> -0.3186360 </td>
   <td style="text-align:right;"> 0.2815230 </td>
   <td style="text-align:right;"> 1.0000000 </td>
   <td style="text-align:right;"> 0.1835504 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> origin </td>
   <td style="text-align:right;"> -0.5514695 </td>
   <td style="text-align:right;"> -0.5862175 </td>
   <td style="text-align:right;"> -0.6335954 </td>
   <td style="text-align:right;"> -0.4747649 </td>
   <td style="text-align:right;"> -0.6039929 </td>
   <td style="text-align:right;"> 0.2275221 </td>
   <td style="text-align:right;"> 0.1835504 </td>
   <td style="text-align:right;"> 1.0000000 </td>
  </tr>
</tbody>
</table>
