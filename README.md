## Natūralios kalbos apdorijmo projektas : Text summarization programa, kur vartotojas gali pasirinkti kokio ilgio santrauką jis norėtų gauti.


 ## Atliko Mantas Kraujelis ir Arnas Mažutis ISI 3kr.

 ## Reikalingos biliotekos

 ```javascript
 pip3 install beautifulsoup4
 ```
---------------------------------------------
 ```javascript
 pip3 install lxml
 ```
 ---------------------------------------------
 ```javascript
 pip3 install nltk
 ```
 ### Paleidimas ir naudojimas
 
  ```javascript
 py summary.py
 ```
  ---------------------------------------------
#### Veikia tik su straipsniais anglų kalba
 ```ps
 Ikopijuokite pusalpio kurio santrauka norite gauti URL adresa: <URL>
 ```
   ---------------------------------------------
 ```ps
  Iveskite norima gauti santraukos sakiniu kieki: <int>
 ```
### Programos veikimas

#### Straipsnio nuorodos gavimas

Naudojama teksto ivestis gauti ```WURL``` kintamjam su URL adresu, adresas apdorojamas naudojantis ```urllib``` moduliu, teksatas gaunamas ir konvertuojamas į naudojamą naudojantis ```beautifulsoup4``` ir ```lxml``` bibliotekomis
 
#### Išankstinis apdorojimas
Pirmasis išankstinio apdorojimo žingsnis - pašalinti visus nereikalingus laužtinius skliaustus, jei jų yra (specialiai skirta, jei norima naudoti Wikipedia strapsnius), tekstas be skliaustų saugomas ``` article_text``` objekte, kiti ženklai, skaičiai, specialūs simboliai kol kas nepašalinami, taip daroma todėl, nes norima vėliau šį tekstą panaudoti sudarant santrauką. "Švarus" Tekstas su pašalintais tarpais (whitespace), specialiais ženklais, skaičiais yra saugomas ```formatted_article_text``` objekte.

#### Teksto konvertavimas į sakinius.

```thearticle_text``` kintamasis naudojamas  tokenizuoti straipsnį, nes jame saugomi full stop'ai (nereikšmingi žodžiai). ``` formatted_article_text``` kintamasis nesaugo skrybos ženklų, del to negali būti konvertuotas į sakinius.

#### Svertinio pasikartojimo dažnumo radimas 

Norit surasti žodžių pasikartojimo dažnį, naudojamas ```formatted_article_text``` kintamasis, jis naudojamas nes jame nesaugomi skaičiai, skyrybos ir/ar specialieji ženklai.

Angliški stopwords saugomi ```stopwords``` kintamaje naudojants ```nltk``` biblioteka. Tada leidžiamas ciklas norint patikrinti ar yra stopwords, jei ne tikrinamas ar žodis yra ```word_frequency``` objekte, jei ne žodis pridedamas į objektą ir jo vertė tampa 1, jei žodis jau buvo pridėtas prie vertė atnaujinama ir padidinama 1. 

Tada norint rast svertinį pasikartojimo dažnį, visų žodžių suma padalyjama iš dažniausiai pasikartojančio žožio pasikartojimų sumos.

#### Saknių rezultatų apskaičiavimas

Sukuriamas ```sentence_scores``` objektas. Objekto raktai yra sakiniai, o reikšmės sakinio rezultato vertė. Tada vykdomas ciklas per kiekvieną sakinį ```sentence_list``` objekte, kur sakiniai tokenizuojami į žodžius. Tada tikrinama ar žodis egzistuoja ```word_frequencies``` objekte, tai daroma, nes ```sentence_list``` objektas sukurtas naudojantis ```arcticle_text``` objektu, o žodžių pasikartojimas apskaičiuotas naudojantis ```formatted_article_text``` objektu, kuriame nėra stop words, skaičių ir t.t. Sakinio rezultatas skaičiuojamas tik sakiniams, kurie ne ilgesni kaip 30 žodžių, tada tikrinama ar sakinys egzistuoja ```sentence_scores ``` objekte, jei ne,  jam suteikiama pirmo sakinio žodžio svertinė pasikartojimo dažnumo reikšmė, jei toks sakinys jau yra objekte, reiškmė pridedama prie esamos reikšmės.

#### Santraukos gavimas

Norint sudaryt santrauką, spausdinami  ```WSentences``` (kuriuos pasirenka vartotojas)  sakiniai su aukčiausiu rezultatu iš N sakinių esančių ```sentence_scores``` objekte. Spausdinimui naudojama ```heapq``` biblioteka ir jos funkcija ```nlargest``` skirta gauti ```WSentences``` skaičiui sakinių.



