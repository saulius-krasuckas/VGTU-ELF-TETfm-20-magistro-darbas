### `aruslt`

```
Date: 2020-11-26 15:37:00 + 29:36s  
Via: aruslt  
```
> Tokiose temose būtinai siūlau Versus principą.  
> 
> Pvz. MPTCP vs L3 srauto balansavimas:  
> 
> ar MPTCP kažką išsprendžia,  
> ar tik randi, kad palaiko (čekboksas Linuksui, ir nieko naudingo kitiems)?  
> 
> Tarkime, turim, schemą: L2 vs L3 vs L4 balansavimas: čia taip, čia taip, čia kitaip.  
> 
> Kaip mes vieną kanalą bloginam? Gal turim Corporate durnos politikos firewallą? Ir jis ima bloginti ryšį.  
> 
> Išvada: reikia devaiso, kuris supranta, kad čia yra MPTCP. Arba reikia
> kvalifikuoto inžinieriaus, kuris išmokintų pvz. Fortį suprasti MPTCP.  
> 
> MPTCP turi papildomus flagus, kuriuos Corporate politika įprastai dropina (ar bent jų dalį tai tikrai).  
> 
> Dirbti reikia išvadoms:
> 
> * privalu išlaikyti ir MPTCP, ir DC akcentus.
> * kai abu fiksuosi, galėsi karaliaut.
> * bus įvairovė.
> 
> Vienas iš tyrimų: HW ugniasienių tyrimas su MPTCP.  
> Galima prasukti DC per VMus VMware: kaip NSXas (internal sw) tvarkosi su MPTCP.  
> VMware palaiko 10G, du servakai RAC synkinasi, per mažai.  
> Padarei 2x NIC ir tikrini: gal pasirodys, kad serverio Backplane nepaveža.  
> Paleidžiam tą patį ir per KVM/Proxmox.  
> Pvz. Checkpoint ant VMware kaip praleidžia?  
> 
> "Pasidarom plytų, tuomet žiūrėsim, koks gaunasi namas"  
> 
> Per daug grafikų tikrai nebus: yra priedai, į kuriuos telpa kad ir 400 psl. (ir jų niekas nežiūri).  
> 
> M1: Išsiversti kelis mokslinius straipsnius apie MPTCP našumą.  
> Kodėl ne TCP? Nes nuobodus (apibrėžimas), nes daug matematikos ir pan.  
> M2: Tiesiog konfiginu MPTCP tarp dviejų mašinų, prasideda malonumas.  

```
Date: 2020-11-27 21:18:00 + 06:27s  
Via: aruslt  
```
> Vienas iš uždavinių yra heretogeninė struktūra: kur vieni palaiko MPTCP, o kiti ne.  
> Tada statom TCP proxy. Galingos implementacijos yra dvi: Nginx ir HAproxy.  
> Tada Windows kompas šnekasi su kitu serveriu (pvz. MS SQL).  
> Statai prieš vieną Proxy, ir prieš kitą Proxy.  
> 
> Tarkim, kad klientas viename DC, o servisas kitame DC.  
> MS SQLas duoda pasileist Demo 30d. Susirandam sintetinį SQL testą ir žiūrim, kas gaunasi.  
> Galima palyginti ir Windows + Linux sqlą (ir pašiepti Windows;)  
> 
> Kiekvienas scenarijus tau generuoja išvadą.  
> Atsiras 17 išvadų. O tada taps be problemų gintis darbą.  
> Kai įbrendi į nebraidžiotus vandenis, užtenka vos vos pabraidžioti, ir lengva padaryti įspūdį.  
> 
> Pagrindinė mintis paprasta: MPTCP vs L2/L3/L4 įranga.  
> 
> Vienas dalykas yra iperfu tarp 2 DC pamatuoti.  
> O visai kitas yra realistinį SQL srautą pamatuoti -- atsiranda visai kitas žavesys!  

### `arustt`

```
Date: 2021-01-23 16:11:00 + 37:01s
Via: arustt  
```
> Vienuose straipsniuose MPTCP pagerina rezultatus, kituose pablogina.  
> Straipsniuose dažnai nagrinėjami du DC su skirtingais dviem NICais:  
> Tikėtina, kad toks setupas ir duoda pablogėjimus / neaiškumus.  
>
> Lengvesnis tyrimo kelias: MPTCP veikimas, kai naudoji heterogeninius tinklus: Wi-Fi, Ethernet, LTE.  
> Kitas tyrimo kelias: kai DC tinklas apkrautas arba su praradimais/prastas, tada MPTCP gal irgi praverčia.  
> Nors atsiranda ir pavojų: pvz. jei paleisi 20 plačių strymų per DC, užmuši kaimyną.  
>
> Galimo tikslo pavyzdys:  
> 
> Ištirti MPTCP protokolą kintančių parametrų tinkluose ir duomenų centruose:  
> ar lyginant su TCP ryšys gerėja, išlieka toks pat, ar prastėja?  
> 
> Atvejai:  
> * vienas NIC apkrautas, imi kitą.  
> * Wi-Fi ryšiui būtų naudingas vartotojui - prisijungi papildomą WLAN IF ir išauga ryšio stabilumas.  
> * 2-3 skirtingų tiekėjų LTE interfeisai suteiktų ryšį ten, kur anksčiau nebuvo įmanoma, pvz.:  
>   - IPTV SLA išlaikymui;
>   - gal net užtikrintas ryšys kariams.
>
> Tuomet uždaviniai būtų:  
>
> - Ištirti TCP ir MPTCP duomenų perdavimo spartą duomenų centruose ir (pvz.) prieigos taškuose (AP).  
>
>   (Dropų nėra, gera pralaida, tiriam standartinę situaciją + 2 linkus + 3 linkus)  
>   (Arba netgi kelis poruojam su LACP ir stebime, ką tai pakeis)  
>
> - Vėlinimui (delsai, angl. Latency) jautrių programų/aplikacijų palyginimas TCP ir MPTCP atvejais (privalumai, trūkumai):
>   - žaidimų multiplėjeris,
>   - video strymingas,
>   - VoIP.
>
>   (Kokią delsą gauname tarp siuntimo ir gavimo?)  
>   (Ar agregavimas per Proxy įneša papildomos delsos?)  
>   (Bet šitą būtų galimai sunku pamatuot, jei aplikacijos lygmenyje / keičia jos kodą)
>
> - Ištirti TCP ir MPTCP energetinį našumą (energonašumą, angl. Power-efficiency) ir palyginti:  
>   ar mes naudojame daugiau resursų tam pačiam srauto pralaidumui pasiekti?  
>
>   (Pvz. 2 korus naudoji kone tiek pat ir gauni daugiau Mbps, t.y. aukštesnį energonašumą)  
>   (Pvz. 2 korai suryja 50% o pralaida išauga tik 20%. Energonašumas krinta)  
>
>   (Visiems tyrimams reikėtų pamatuoti ir sunaudotus CPU + RAM resursus: darau tą ir tą, turiu tokį ir tokį efektą)  
>
> - Apibendrinti ir palyginti abu protokolus: TCP ir MPTCP.  
>   (MPTCP yra TCP protokolo pratęsimas)
>
> Papildomas uždavinys:
>
> - Ištirti klaidų atsistatymo (perkrovų valdymo? angl. Congestion Control) mechanizmus.
>
>   (Bet čia sudėtinga, gilu)  
>
> Papildoma tyrimo sritis:
>
> - MPTCP Proxy panaudojimas keleto tiekėjų agregavimui:  
>
>   Pvz. darbuotojas dirba per VPNą, o VPNas srautą nukreipia tik per vieną NICą.  
>   Nurodai, kad šis srautas eitų per MPTCP Proxy ir per iškeliautų į galinį tašką.  
>   Šiame taške irgi stovi MPTCP Proxy.  
>   `saukrs`: "Abejoju, ar būtina statyti MPTCP Proxy galiniame taške. Juk tai Multipoint-to-Point technologija".  
>   `arustt`: "Tampa patogesnis MPTCP tyrimas / matavimas".  
>
>   Bet tuomet išauga laboratorinės MPTCP infrastruktūros kaštai:  
>   Vienu metu naudojame kelių 4G ryšio tiekėjų paslaugas ir LTE NICus.  
>
>   Vienas iš panaudojimo scenarijų hibridinis:  
>
>   Panaudoti 5G (didelį pralaidumą) kaip atsarginį duomenų centro Uplink ryšį (lygiagrečiai optinėms skaiduloms).  
>   Tinka avarijų atvejams, kai tiekėjas trumpam nutraukia ryšį.  
>   Galbūt tiktų ir kai užkemšamas duomenų centro Uplink pralaidumas.  
