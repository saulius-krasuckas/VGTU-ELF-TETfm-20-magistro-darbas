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
> - ištirti tcp ir mtcp duomenų perdavimo spartą DCse ir prieigos taškuose (AP).  
> / dropų nėra, gera pralaida, std situacija + 2 linkai + 3 linkai (arba netgi kelis poruojam su LACP)  
> - latency / vėlinimui jautrių programų/aplikacijų tyrimas (geiminas, strymingas, VoIP) tarp TCP ir MPTCP (privalumai, trūkumai).  
> / kokį delėjų gauni tarp siuntimo / gavimo.  
> / ar agregavimas per proxy įneša delėjų (bet šitą galimai bus sunku pamatuot).  
> - TCP ir MPTCP power-effieciency tyrimas:  
> / ar mes naudojame daugiau resursų t.p. srauto pralaidumui pasiekti.  
> / pvz. 2 korus naudoji ir gauni daugiau Mbps (ir efektyvumą).  
> / pvz. 2 korai suryja 50% o pralaida išauga tik 20%. (ir pirmu, ir antru tyrimu:CPU. RAM. turiu tokį ir tokį efektą)  
> - apibendrinimas ir palyginimas abiejų protokolų.  
> (- tiriame klaidų atsistatymo mechanizmus, bet čia sudėtinga)  
> (- mptcp proxy for multiprovider aggregation)  
>
> darbuotojas dirba per VPNą, VPNas dirba tik per vieną NICą  
> nurodai, kad eitų per proxy ir patektų į galutinį tašką, kuriame irgi stovi MPTCP Proxy (bet gal ir nebūtina PRODui).  
> Išauga mptcp labės kaštai (keli 4g if; arba 5G kaip bekupinis DC linkas šalia optikos).  
