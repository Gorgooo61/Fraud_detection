# Fraud-detection
## Feladat leírás
### Business tasks:
A feladat csalók, illetve nem fizető ügyfelek felismerésére képes mesterséges intelligencia modell készítése.
### Buisness goals:
A fentebb említett ügyfelek pénzügyi veszteséget okoznak a cégnek. Ha már a szerződéskötés pillanatában ki lehet szűrzni ezeket az eseteket, azzal pénzt lehet spórolni. A rizikófaktor tekintetében különböző csomagokat lehet létrehozni, és ajánlani az ügyfeleknek.
### Hipotézisek:
- Az alacsony kezdőrészlet (különösen magas teljes készülékár mellett), alacsony életkor, magas havidíj növeli a csalás esélyét
- Részletfizetéssel vásárlók körében nagyobb a csalás aránya
- Bizonyos gyártók, és készüléktípusok esetében magasabb a kockázat (elsősorban prémium termékeknél)
## Adatelemzés:
### Adatelőkészítés:
- Virtuális vásárlások OUTLET_COUNTY_ENCR feltöltése az ADDRESS_COUNTY_ENCR adatokkal, hogy feleslegesen de töröljük ezeket az adatokat
- Ahol nem volt részletfizetés, és hiányzott az INSTAL_CNT, ott 0-val való feltöltés
- Hiányzó operációs rendszerek kezelése (4 olyan model, amihez nincs információ, manuálisan kitöltve)
- További duplikált és hiányzó adatok törlése
### Adatelemzés(EDA):
- Mindkét célváltozó szerinti százalékos metrikák készítése, outlier vizsgálat (lásd a kódban)
- PRODUCT_NAME eldobása a nagy variancia
- Numerikus konverziók, correlation heatmap
-  Megállapítás, hogy minden nopay_after_12month pozitív ügyfél "csaló", hiszen nincs olyan, aki azért nem aktív fizető, mert lejárt a törlesztő ideje
-  A FRAUD_STATUS_6MONTH pozitív ügyfelek a nopay_after_12month pozitív ügyfelek egy részhalmaza, ezért időhiány miatt innentől nopay_after_12month-t célváltozóként haszálni
-  Feature engineering: device_discount, real_monthly_cost létrehozása, továbbá a célváltozóval gyengén korreláló, vagy egy másik feautre-el erősen korreláló feature-k eldobása
## Modellezés:
### Metrikák:
- Classification report
- F1 szerint optimalizált threshold
- Confusion matrix
- Feature importance
### Modellek:
#### Dataset további módósítása nélküliek:
- Random Forest Classifier
- XGBClassifier
- XGBClassifier, GridSearch-el optimalizálva<br>
Egyik modell sem ért el túl jó eredményt, különösen a sok volt a false negative
#### Két külön xgb modell az  INSTALMENT_IND szerint
- nem hozott szignifikáns javulást, erős regularizácó mellett sem
#### Az előző megközelítés használata más feature selection-nel
- nem hozott javulást
## TODO:
- Más modellek kiproálása
- További feature engineering
- Egyéb optimalizációk, esetleg f1-score helyett kifejezetten recall-ra optimalizálni false pozitív értékeket bevállava
- modellek FRAUD_STATUS_6MONTH feature-t célváltozóként használva
