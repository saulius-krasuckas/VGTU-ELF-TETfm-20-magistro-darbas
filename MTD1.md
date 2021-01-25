### Planuojamas magistrinio darbo tikslas:

Ištirti literatūroje rečiau aprašomus MPTCP protokolo naudojimo scenarijus (t. y. duomenų centruose).  
Palyginti su populiariais naudojimo scenarijais (pvz. Wi-Fi + LTE, DSL + LTE).  

### Į tikslą vedantys uždaviniai:

* Ištirti TCP ir MPTCP duomenų perdavimą spartą duomenų centruose (ir pagal galimybes prieigos taškuose, AP).

  DC: nėra paketų praradimų, aukšta tinklo pralaida, tiriama standartinė situacija, 1 NIC.  
  Ar čia panaudojus MPTCP ryšys gerėja, išlieka toks pat, ar prastėja?  
  Tuomet į tyrimą įtraukiami 2 NIC, tuomet 3 NIC, galiausiai 4 NIC.  
  (Pagal galimybes panaudojant LACP ar kitą agregavimą.)  
  (Pagal galimybes palyginti protokolo elgseną duomenų centruose su elgsena kintančių parametrų tinkluose.)  

* Vėlinimui (delsai) jautrių aplikacijų elgsenos palyginimas TCP ir MPTCP atvejais.  

  Aplikacijos: video strymingas, VoIP ryšys, žaidimų multiplėjeris.  
  Kokią delsą gauname tarp išsiuntimo ir gavimo įvykių?  
  Ar agregavimas per MPTCP Proxy įneša papildomos delsos?  
  (Pastarasis pagal galimybes: galimai būtų sunku pamatuoti aplikacijos lygmeniu / jei tektų keisti jos kodą.)  
  Kokie MPTCP naudojimo privalumai ir trūkumai?  

* Ištirti TCP ir MPTCP energonašumą (angl. Power-efficiency) ir palyginti,
  kaip naudojant MPTCP keičiasi reikalaujami skaičiavimo resursai.  

  Vienas atvejis: CPU du branduolius naudojami vos keliais procentais daugiau, o pralaida išauga kartais.  
  Kitas atvejis: CPU du branduolius naudojami po 50%, o pralaida išauga tik 20%.  
  Į matavimą vertėtų įtraukti ir RAM resursus:  
  scenarijus keičiamas taip ir taip, resursų naudojimas kinta kitaip ir anaip.  

* Ištirti, kaip minimi MPTCP rodikliai/aspektai keičiasi pereinant iš fizinės infrastruktūros į virtualią.  

  Omeny turima virtualizacija: hipervizoriai / Virtual Machine Managers (VMM).  
  Dėl dešimtis kartų išaugančios virtualios pralaidos tikėtini neigiami reiškiniai su CPU resursų išnaudojimu: ar tikrai taip?  
  Ar Virtualių mašinų (VM) CPU laiko Scheduling algoritmai / niuansai irgi paveiktų pralaidą?  

* Apibendrinti ir palyginti abu protokolus: TCP ir MPTCP.  
  (kadangi MPTCP yra TCP protokolo tąsa, suderinamas tinklo lygmenyje)  

Papildomas uždavinys būtų:

* Ištirti galimybę panaudotiu MPTCP Proxy keleto tiekėjų agredavimui.

  Tai arba kelių LTE tiekėjų paslaugų apjungimas, arba pvz. duomenų Uplink optinio kanalo apjungimas su 5G.  
  Pirmasis praverstų VPN ryšiams, kurie įprastai veikia tik vienu tinklo interfeisu.  
  Tam reikėtų nukreipti srautą į MPTCP Proxy.  
  Antrasis aktualus avarinėms situacijosm, kai tiekėjas trumpam nutraukia ryšį.  
  Galbūt net ir kai užkemšamas duomenų centro Uplink pralaidumas.  
  Toks uždavinys labiau pakeltų laboratorinės infrastruktūros kaštus.  
