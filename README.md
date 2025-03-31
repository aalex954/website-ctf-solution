# website-ctf-solution
Solution to the two CTF puzzles embedded in the personal-site

## Part 1: Moorse Code SVG (fake)

### Alt Text Hint

Inspect the HTML element to find a hint in reference to the youtube url structure.

```html
<img src="/static/img/svg/morse_oscilloscope_style.svg" alt="I forgot to encode the = symbol so youll have to add it back ^_~">
```

### Convert the Moorse code to text

#### Zoom in on the sine wave graphic to reveal dots and dashes. 

**OR**

#### View the Moorse2SVG Project 

```/posts/steganography-moorse-converter/``` to find the original Moorse string hardcoded into the post.

### Decode the Moorse Code to reveal a partial flag.

```"..-. .-.. .- --. ---... -.-- --- ..- - ..- -... . .-.-.- -.-. --- -- -..-. .-- .- - -.-. .... ..--.. ...- -.. --.- .-- ....- .-- ----. .-- --. -..- -.-. --.-"```

#### Extract and Fix the URL

```FLAG:YOUTUBE.COM/WATCH?VDQW4W9WGXCQ```

```https://www.youtube.com/watch?v=dQw4w9WgXcQ```

### Interpret the rick roll to be a dead end and continue.


## Part 2: Library of Bable (real)

### About Page meta tag

#### View the about page source to discover a meta tag named hint with a base64 encoded message.

```xml
<meta name="hint" content="Q29udGVtcGxhdGUgdGhlIHZhcmlhdGlvbiBvZiB0aGUgMjMgbGV0dGVycw==">
```

Decoded: ```Contemplate the variation of the 23 letters```

#### Google search the string to find the library of bable.

Recognize the need for an index. 

### SVG metadata

#### Inspect the contents of the svg file. 

Discover doule base64 encoded ```CDATA``` in a custom metadata sub tag called ```bable:book```

```xml
 <metadata>
  <rdf:RDF xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:cc="http://creativecommons.org/ns#" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
   <cc:Work>
    <dc:type rdf:resource="http://purl.org/dc/dcmitype/StillImage"/>
    <dc:date>2025-03-27T01:11:55.342307</dc:date>
    <dc:format>image/svg+xml</dc:format>
    <dc:creator>
     <cc:Agent>
      <dc:title>Moorse Code Encoded SVG Image</dc:title>
     </cc:Agent>
    </dc:creator>
   </cc:Work>
  </rdf:RDF>
  <babel:book xmlns:babel="http://libraryofbabel.info/ns#">
      <![CDATA[
         Vm05c2RXMWxJRE15SUc5dUlGTm9aV3htSURJZ2IyWWdWMkZzYkNBeklHOW1JRWhsZUdGbmIyNE5DZzBLTUd4a2JuRmlNMkZ0WnpKcmFXOXZjelJvTkRZMGEzSTBjSEZwTW5ZMGJYVm1jV05sT0RGaGJ6aDBNelZ2YlRsek5IWnJlWG80Y21wa2VuQXpObXh5ZFdOdE5IQjJlV1psY0c4NGNtWXdObWhwTkhObFpEQm9OSEkxY0dkM2JuYzBOMlptY3poNWRucDRkMlZuY0RsblpqUTVNSFJrWnpGbmJHczRaV2wyYUdOM2NHeHZlblZoT1dscmVtWnZhamgwYkRCeGJXdGpaV0kwZW5ock1Ib3hiV2t6YzNOaWJqSnVkMkp3ZUdocWVqSjNjekUxT1c5d05YaHFhRzEzZW1kcVluTjRabkp0Y0hka1ozWnRlV1ZtYjI5dGQyNXFZemxyY0hOemFqUnpOVFZ2ZERWMU1HVjVaMkZqWW1GamRXVjRZako2Y2pZMmRHeDNiVzVrWTNWdmVHZzFObTEzYlcxaU9IRjRabTlzYkhGME5YSnlPVzVtY0RJd2NqTnBkMkZrTW1scFlqUm1iRFpzT1dadE9YQTRabk56TVRScmMyVm1kemh1Wm5Wc2VtRmlNbk4wY1dad2VEZDNkRGx1TTJkeWRtVTBjREp1WjNneWNXUTFNamt6YTJSeE9YZzFlbmRrTm10ME1uSjBkMmt6T1dzMWFXcGpZV1E0ZFdwMmNYUjVibXA0TnpRd05YbzJkbWsyY21SdE0yRjZNMkZ2YW5Kdk5tVmtkalJ2ZHpsd1lXOWtkSFpvTjNaMGFtNWhabWczWjJSdWRIQnVZM0J3Y2pVeWJucGtZVFJuYnpNd1puSnBNSHBuYUhGaGR6QnhkMnN6Tm1wcGJ6bDZOR2N3Wm5wNmFqaHJjekV3Tkc0eE1XOXhiMlIyTmpkcWJtSXlhM1ppY2psd2RUVnhiMmQwYkdWNWQzbHhjSFYwYVRBd2VHZHdkamg2YW5ZNVpHTmtkakZ5ZWpRemVtMDRlSGw1TUdOemFXUnJNV04zWjI1d01EZG5ORGxoTW10cGIyUnZkWGM1T1dsM2FYazFZMk5rZW5ZM2VHbDVaV2wwY0dGcmNqUm1Zbk52TW10cE1tTnRjWGd6ZEhJME5tTnRPR1Y0TlRCNmNtOWlZbmt5TW5WdU5YbDBjMmw1TTJGdmVIVjNZM1o1YURVemVuUXlOM0JwY2poaGF6TXhaM00yYldwc05UQjJjV051YjJReU9Xa3hOM2wxYTJWeE0zTjFaMjFuTURFelptaHBlVzFzYW01amVEQTFaVzEzZHpoMU9EWjNNVE5vWkRGc2IyRTNaVEEzYW00NWFXSnlZWFF3TWpVMWRHRnNObU4yYTNwMk9HTm5jblEwWW1zMGRHZzJjemgwTmpjNGNqRmlNbkIyYURCeGQyYzNNalZtY21Sa2IzSjFiM2RxTkhabGFESndlV0ZoZEdWbE5HaHFNR0ZoYURKNFluVmthRzUzWTNscU1ETTBZbUpyY0dGellXNTZOalkyZWpsd01uZHliWFE1TlRRMmJubGhhVEExWW5salpYUXdkM2hyYkRkdmJYVmhkek16ZW1oaGNqazFlV2hzTkdkaWNYaHdjV1UyTUdwMmJEUjJaekp5ZFdsdk5UZDBhbTkwYlRCM2VuVm9aV0ZqTURkMk9IazVaVzVpYzJsNWJHOWhOemxpZDJrME0zbDFNbU0xYzNGMGVHRTRablJpTTJobk9HMW5NWEV6T0hSblpIWXlZbVZrYW01a2JtTnFZbXBsYVdsNWNqSTFNM1ZoTjNVeU1HYzFiREkwYW5sM1oyeGhhM2hwYUd4emIzcG1hREJwTkRkdGFHb3llbXhvT0dwMWJ6WTFlbTQ1TW5Sb2FuVjZiM1Z3Ym1sbWNIcGtabXQwTmpSdmFHMXBOVGt5TTJoamVYUjFjSGRyTjI5bU9IcDRibkZ0TjJoNmJITmtjWE5yWjJkNmIyVmtjRGR3WkdVemMzRnBiM1JzYm5aMWJtUndaM3ByYVd4b09XZHZjbWcyZVRNMU1tZDFlbW81T1d3M2R6ZzJjWFl6Ykhjd2FHMXlkV3BwWldGMk5UTjFOREp2WWpaaWNHdzVaMlV4WW1GbmVXVXlhVzAxWm1wdE1tbG1PSGQ2TTNaNU9HdzVhR0pxYWpSa01IVTBiM1JuWTNGemMycHBiV1YyYzJKbmRqVjNkVFZrTVdVemJtTXlPWFZ5YVdJelkzTm5kM3B2YkhrM2FEY3hjMlJqTW5kNE9UYzNiR1ZyZEhBd2MyeHdiMmcxTlhVek5UTmplV056Y1d4eU9XVm5ZVGgzTXpKNGNHZDJNak54TVhGM2RHeHBaVzFpYkcweE0zcDJlVGxyZFdGak4ybzNhSGg2WW01M01XUm9iek5sZHpVM1oyRTROakk0ZFdweloySnJaMlJ2ZFdkeGEydzNNV2RpWjJoNWRYZHNiMlZwZURCME9HZHVNR1I0ZW5CMWFXVm5abmhqYTJjME0yeGlZVGRsZW5jeGJESTViRzVyWXpjeWFqaHNaRGgxTVRoNE16aGhZVEE1ZG5oemRuUjZlV1p4Ykc0MGEyeG1hek5uTW0wNWNUTnNiRzkzTUhrd1lXeHRNakEwTUdnME1ERnZhMmc0WnpSaWVIZG1jVEoyTTNWd2RHUjNkVGwzTkcxcE9EZHlkMjQ1T0hSbU5XbDJZbTk2WnpGdE1HOTJOSHBoYkhOeWMybDRlSFp5ZG5RM2JIVnhOSE5yWW5ZME1EazBhMkZyYzJwdGFqbDViV051YjI4d2JYRjRNblp6YW5SMFoyOTBlakprT1hBM2J6VjJPREk1WVhWeWJuSndNRGx2T1RaaE5uUTVNM1ZsTTNadlluVnROVEJ5YkdSNFpuZ3pjekE0WmpOdlpYWmxkV2N6T0dGeGNYSmxaR0ZpTW5JM01tNXdiM014YzNBMWVuZHFabVpvTVdvNWQyODNlR3BvTUdWd056ZDJhRzkzWlhSdFl6UnhhbVoxYVRSelpXVnpZbWsyYUhBNGNtUjBNV2RoTjI5MVpXbHhialF5YlRWdU1tbHdObWxuTnpnMWVqRnJhblEyWmpscU1XVm1kVGhyTjJKMWNUQXdkSFEwWm5Cd1pIQjVNR1pzZG5ZeVltdzRhemQwT1RSNWRHbG5hbW93YTJsbGJ6UmtOelV3YURRNFlXWjJkblZ5Wm5wemNUZG9kVFp2WVhSbVlUVnRkR2N4YWpWbE9YUjZaamd3YUhadk1YVjBiR2hxZVhSbU1YTXlNWFJ5Y0Rsdk5XUnBiSE5pTmpaM05XTm5aR1p2YUdkM1lqVnZjbVJ2TkRkcmFXNTZjMnQwY2pac1lqQnFiMk0zT1hwaWNEQmlkRE5vYVhRM2VqaHFOM0JqYXpBMWVERmpPSEkwT0RCMU1qa3pkV0p1TVhsek5HODJjekJ6WkhWc01XODFZbkExWm14NE4yOXVPWHBzYzJkeVp6VTVZbVo2T0RKbmFuWjBiVzh6ZUhreGFtaHRNR3N6TnpONmJXNTNiV3N6WkRadmVUTjVlVFYxWWpBeWNHOHllRzB3TUdNNWRuSnJiMlo1TW1WNFozSnpPVFU0WkdGa05HcG9kSEpvYm1GeWFIQXlhM0JxWmpkcE0zWmpiM2gzY1c1ck9HUmpaV1JoWm1WdWFHNXBPSGh3Wm05NVozWXlNekUxWVdNMVlqQmhabVZ0WXpObk9HRXlOWGxyYjJoaFlqRmtOMlZxTlRGbmJ6UXpZV2MyWVhobU5HbHdlVzk1T0Rsek56Rm5lVEp4WVdOMU1qSmhZbVJ5WjJ4eGRtTmllbU51YURSclpXZ3piSGN3Tm1WNE9HNW9halJ1WWpZMGMzTXlOWEE1YW1jM2FUSjJkelJ2TVRGdWFHdDRabmwwYm5Sa2JUTnNZWFprYkc5dU9YWTJOREU0Ym01MWRIUnRjM0J3Ym1kM2FHb3hkelZxTW1WemVtMWxjelkwTlhNeU5qTXhaVGwxWW13eE1Ha3liSE0zYUdscE5HTm9iM1poTm5KM2JuTm9NMm96WVhJd09XbGxiVFEwZW1OdmFUUnpjMm81YzNKNk9USnhOR2RpYXpSamRYZGhZbkpqWTNjek9HTnROM1ZuYTIxMGQzTnFjV2xtWjNKcVkyZHVPVGN5YzJzeWFUSm9ZbTE2T1RCbWVHeHBOM0ZqTm1OMGNXWXpOV0ZvWVRkaVpYZ3lZblYyTUd0NGFHWjJOek5xY1dzMlpXNDVlRzloTlc1bWFXSjFOR0l5Y1hSbVltRTBhRzl3WVdKeE1uQjROVFZtYlhoa2JITnpaR1Z4TkdSemNtVm5kV3N4T0d4NmFHUnViWGxuZG5CNGF6bDNabkp6YTNJeE5ERjNNMjl1WVdwNllYSnRPSFJzTkRnNGJHSnBNemh1T0RreGVtazFhbk15YVhGa05UQm9ORE00WmpneFltazNaSGwwTTJscmRXcDJOVzl4ZW1oc01YaDZhV2h2YjNremN6TmtOREp6YjNOaE0zbDNjV296WTJKNGRUTXhPREJrYVRkdU1tSTJiV1l6TUdZek5ESXljekF5Tm1Wc1pqZDBkM2t5TVRNd1pHRmpNR2MxYVhReWFITXpOWEpwY3pnNU56RmpiVEIzWWpOM2VqQmlaMkYzZFdVellXMWpaM2g0ZDNsak56WjBOVEJwYVdSak16VjRPR00yTld0MWQzVXhiemswYXpOcGNEVjNkekJ4Ym1Ga056TjRPRGxoTWpjd1pqTmpiakEyTlhOMmNHRmtiamc1T0dZMWRXc3daMm8xTkhjemJtTm9kMnhqWm1keVp6aHROblU0TkhWNmQyY3daMmh3Y25sb2JXOXNhakowY25reWNHWTJPVE0wTTI5bWRITnNhbTV2Tkdod2JuQTBhM0oyWjJwNGMySnpiVFEwZG0xdGNXVTJZekpsZG5KMVpURTRhamxxWlhoeWRHZGxNR0Z3YTNWcmFtTnhPVGxvYm5Kd2RYbDNlSEEzT0ROdWFEVnVOalk1YURKbE5XUjRjbVU0ZG1kemIyUjBkRGR1YWpaak1XTjJZak0yTjJjM2FESTFjVzl4Y0d0dFozQmhiV1YzWjJod05qUjNhMjFuWkc5b2FtMW5OSEZzT0doNE5tOHlObnB2WVRRM01XRTVaVFF4ZDJjeWFubG1NWEo2TlhKMU9XRm9ObkowTVhneGVtOXdibUZqTjNOaWNEQTBlV1JvWnpWM1lXcHFaSGd6Wkhsa1lteG1abWd4T0RVeGVUZzFjREZsYkd3ek1HNXBOSE0xTmpCbGMzVm1aM1F6Y21SdmNXSjNibXgyYjIwNGFtTTVObkZ5YkhSdWVXMW1kSEZsWkdweE5IZHFZbUozZEdGbWEyWnFNalpvYm13eWNXMDJhV3MwYkRrMGJYUTFlamh2TUdwbWVEWmxZekEwTkRObllYcHpkbVZ3ZDNrelp6WnhObWQ2YjJSME0zQnJPRE51TnpodllXTjBOVzl6Ym1od2QzWjROV3hrTUdKNmNIbDVZbWRrYmpOb2VYQm1ZemcxYkhSNU5tczRabWxrWVd4aVl6Rm1ZbVo0Ym00M2FHOXdZVGxuYTJ0cllUSTVhR1JxZEc1a09HNTFjelo1Tkd4NGNYZDViVFpoY0dwNU9EUXpZbXQzY1RVeWNYRjBNMlpxWTNJeWN6WnBNemx1WTJkdVpEQXdNRFJrTlhoalkzUndNMmd6YkhveWEzbHVNalJyWTJZMk5ITTRZWEkyZUdObFpERmtOVGgwWm1neGJIaGxhREYxWVRnMlpqaDNkbkJpWVdaM04yTT0=
      ]]>
   </babel:book>
 </metadata>
 ```

 #### Decode Double b64 Encoding

```csharp
Volume 32 on Shelf 2 of Wall 3 of Hexagon

0ldnqb3amg2kioos4h464kr4pqi2v4mufqce81ao8t35om9s4vkyz8rjdzp36lrucm4pvyfepo8rf06hi4sed0h4r5pgwnw47ffs8yvzxwegp9gf490tdg1glk8eivhcwplozua9ikzfoj8tl0qmkceb4zxk0z1mi3ssbn2nwbpxhjz2ws159op5xjhmwzgjbsxfrmpwdgvmyefoomwnjc9kpssj4s55ot5u0eygacbacuexb2zr66tlwmndcuoxh56mwmmb8qxfollqt5rr9nfp20r3iwad2iib4fl6l9fm9p8fss14ksefw8nfulzab2stqfpx7wt9n3grve4p2ngx2qd5293kdq9x5zwd6kt2rtwi39k5ijcad8ujvqtynjx7405z6vi6rdm3az3aojro6edv4ow9paodtvh7vtjnafh7gdntpncppr52nzda4go30fri0zghqaw0qwk36jio9z4g0fzzj8ks104n11oqodv67jnb2kvbr9pu5qogtleywyqputi00xgpv8zjv9dcdv1rz43zm8xyy0csidk1cwgnp07g49a2kiodouw99iwiy5ccdzv7xiyeitpakr4fbso2ki2cmqx3tr46cm8ex50zrobby22un5ytsiy3aoxuwcvyh53zt27pir8ak31gs6mjl50vqcnod29i17yukeq3sugmg013fhiymljncx05emww8u86w13hd1loa7e07jn9ibrat0255tal6cvkzv8cgrt4bk4th6s8t678r1b2pvh0qwg725frddoruowj4veh2pyaatee4hj0aah2xbudhnwcyj034bbkpasanz666z9p2wrmt9546nyai05bycet0wxkl7omuaw33zhar95yhl4gbqxpqe60jvl4vg2ruio57tjotm0wzuheac07v8y9enbsiyloa79bwi43yu2c5sqtxa8ftb3hg8mg1q38tgdv2bedjndncjbjeiiyr253ua7u20g5l24jywglakxihlsozfh0i47mhj2zlh8juo65zn92thjuzoupnifpzdfkt64ohmi5923hcytupwk7of8zxnqm7hzlsdqskggzoedp7pde3sqiotlnvundpgzkilh9gorh6y352guzj99l7w86qv3lw0hmrujieav53u42ob6bpl9ge1bagye2im5fjm2if8wz3vy8l9hbjj4d0u4otgcqssjimevsbgv5wu5d1e3nc29urib3csgwzoly7h71sdc2wx977lektp0slpoh55u353cycsqlr9ega8w32xpgv23q1qwtliemblm13zvy9kuac7j7hxzbnw1dho3ew57ga8628ujsgbkgdougqkl71gbghyuwloeix0t8gn0dxzpuiegfxckg43lba7ezw1l29lnkc72j8ld8u18x38aa09vxsvtzyfqln4klfk3g2m9q3llow0y0alm2040h401okh8g4bxwfq2v3uptdwu9w4mi87rwn98tf5ivbozg1m0ov4zalsrsixxvrvt7luq4skbv4094kaksjmj9ymcnoo0mqx2vsjttgotz2d9p7o5v829aurnrp09o96a6t93ue3vobum50rldxfx3s08f3oeveug38aqqredab2r72npos1sp5zwjffh1j9wo7xjh0ep77vhowetmc4qjfui4seesbi6hp8rdt1ga7oueiqn42m5n2ip6ig785z1kjt6f9j1efu8k7buq00tt4fppdpy0flvv2bl8k7t94ytigjj0kieo4d750h48afvvurfzsq7hu6oatfa5mtg1j5e9tzf80hvo1utlhjytf1s21trp9o5dilsb66w5cgdfohgwb5ordo47kinzsktr6lb0joc79zbp0bt3hit7z8j7pck05x1c8r480u293ubn1ys4o6s0sdul1o5bp5flx7on9zlsgrg59bfz82gjvtmo3xy1jhm0k373zmnwmk3d6oy3yy5ub02po2xm00c9vrkofy2exgrs958dad4jhtrhnarhp2kpjf7i3vcoxwqnk8dcedafenhni8xpfoygv2315ac5b0afemc3g8a25ykohab1d7ej51go43ag6axf4ipyoy89s71gy2qacu22abdrglqvcbzcnh4keh3lw06ex8nhj4nb64ss25p9jg7i2vw4o11nhkxfytntdm3lavdlon9v6418nnuttmsppngwhj1w5j2eszmes645s2631e9ubl10i2ls7hii4chova6rwnsh3j3ar09iem44zcoi4ssj9srz92q4gbk4cuwabrccw38cm7ugkmtwsjqifgrjcgn972sk2i2hbmz90fxli7qc6ctqf35aha7bex2buv0kxhfv73jqk6en9xoa5nfibu4b2qtfba4hopabq2px55fmxdlssdeq4dsreguk18lzhdnmygvpxk9wfrskr141w3onajzarm8tl488lbi38n891zi5js2iqd50h438f81bi7dyt3ikujv5oqzhl1xzihooy3s3d42sosa3ywqj3cbxu3180di7n2b6mf30f3422s026elf7twy2130dac0g5it2hs35ris8971cm0wb3wz0bgawue3amcgxxwyc76t50iidc35x8c65kuwu1o94k3ip5ww0qnad73x89a270f3cn065svpadn898f5uk0gj54w3nchwlcfgrg8m6u84uzwg0ghpryhmolj2try2pf69343oftsljno4hpnp4krvgjxsbsm44vmmqe6c2evrue18j9jexrtge0apkukjcq99hnrpuywxp783nh5n669h2e5dxre8vgsodtt7nj6c1cvb367g7h25qoqpkmgpamewghp64wkmgdohjmg4ql8hx6o26zoa471a9e41wg2jyf1rz5ru9ah6rt1x1zopnac7sbp04ydhg5wajjdx3dydblffh1851y85p1ell30ni4s560esufgt3rdoqbwnlvom8jc96qrltnymftqedjq4wjbbwtafkfj26hnl2qm6ik4l94mt5z8o0jfx6ec0443gazsvepwy3g6q6gzodt3pk83n78oact5osnhpwvx5ld0bzpyybgdn3hypfc85lty6k8fidalbc1fbfxnn7hopa9gkkka29hdjtnd8nus6y4lxqwym6apjy843bkwq52qqt3fjcr2s6i39ncgnd0004d5xcctp3h3lz2kyn24kcf64s8ar6xced1d58tfh1lxeh1ua86f8wvpbafw7c
```
### Find the Page Clue

```<meta name="page" content="󠁐󠁡󠁧󠁥󠀺󠀠󠁯󠁮󠁥󠀭󠁴󠁷󠁯󠀭󠁯󠁮󠁥">```

OR

```<p><code>󠁐󠁡󠁧󠁥󠀺󠀠󠁯󠁮󠁥󠀭󠁴󠁷󠁯󠀭󠁯󠁮󠁥</code></p>```

#### Extract the hidden text as per this guide

[Hidden-in-Plain-Hex](https://github.com/aalex954/Hidden-in-Plain-Hex)

### Find the passage in the Library of Bable

Using the clue from the about page meta tag earlier:

```https://libraryofbabel.info/browse.cgi```

#### Navigate to the Volume on the Shelf of the Wall of the Hexagon

Use the PAGE (121) found earlier 

**OR**

#### Brute Force By Page

### Conclusion

```bash
Thanks for digging around. For searching. For finding...........................
................................................................................
................................................................................
The most beautiful thing we can experience is the mysterious. It is the source..
of all true art and all science.................................................
................................................................................
Albert Einstein.................................................................
```
