Sammendrag av TTM4130 - Tjenesteintelligens og mobilitet
========================================================

Sammendraget er basert på læringsmålene i faget, og tar utgangspunkt i fagets kompendium og forelesningsfoiler, samt delvis ekstramaterialet spesifisert i læringsmålene.

(Deler av stoffet er kopiert fra ekstrematerialet, Wikipedia, eller andre kilder, og det er derfor mye blanding av norsk og engelsk i dette dokumentet.)

Know the difference between access network, transport network and service platform/IN
-------------------------------------------------------------------------------------

![Grov oppdeling av nettet](https://github.com/kjbekkelund/ttm4130/raw/master/images/oppdeling.png)

* Et moderne kommunikasjonssystem består av et aksessnett og et kjernenett, der kjernenettet gjerne deles i to nivåer, tjenesteplattform og transportnett.
* Transportnett: Formidler nytteinformasjon, "budskapet", mellom deltagerne.
* Tjenesteplattformen blir anvendt for oppsetning, styring og kontroll av transportstrømmene.
* Utviklingen av tjenesteplattformen har kulminert i IN, som standardiserer funksjoner i tjenestestyringslaget.
* An access network is that part of a communications network which connects subscribers to their immediate service provider (tjenesteleverandør). 
* Det eksisterer to hovedutgaver av tjenesterealiseringsnett i Europa:
  * Intelligent Networks
  * GSM-tjenesteplattform
* Både IN- og GSM-rammeverket er uenget når mobile brukere ønsker integrering av webbaserte tjenester med vanlig telefoni, med fokus på multimedia-innhold.

Be familiar with modeling of a normal telephone calls states, and how to illustrate this by state descriptions and sequence diagrams
------------------------------------------------------------------------------------------------------------------------------------

Vanskelig å få tilgang til terminaltilstanden til en telefon direkte, og overvåker derfor samtaletilstanden:

* SBCP (Subscriber Basic Call Process). Ligger i endesentralene.
* TBCP (Transit Basic Call Process). Ligger i transittsentralene, tilsluttet hver enkelt inngang.

Realiseres i form av tilstandsmaskiner. Hver enkelt abonnentlinje må ha sin egen tilstandsmaskin.

![Tilstandsmaskiner](https://github.com/kjbekkelund/ttm4130/raw/master/images/tilstandsmaskiner.png)

Minst tre forskjellige nivåer av informasjon for å overvåke tilstanden i nettet:

* Nettets driftstilstand, inklusive den enkelte komponent.
* Rutingtabeller.
* Den enkelte forbindelse eller oppkobling.

Sentrale aspekter:

* Weak coupling
* Interfaces
* Weak synchronization
* Autonomous fault recovery

Be familiar with the Intelligent Network architecture
-----------------------------------------------------

* IN = Architectural concept intended to be applicable to all telecommunications networks and aims to ease the introduction and management of new services.
* With IN technology it is possible to introduce new services rapidly without affecting the available services. IN defines a large set of standards that describe the interfaces between different network control points. With only specifying the interfaces IN makes it possible for vendor systems to provide with different products and for operators to use any of these products in their network configuration.
* Beskriver interfacene mellom forskjellige nettverkskontrollpunkt => Leverandøruavhengig.
* Intended both for fixed as well as mobile telecom networks. Allows operators to differentiate themselves by providing value-added services in addition to the standard telecom services such as PSTN, ISDN and GSM services on mobile phones.
* The intelligence is provided by network nodes owned by telecom operators, as opposed to solutions based on intelligence in the telephone equipment, or in Internet servers provided by any part.
* Flytter kompleksiteten fra sentralene.
* Based on the Signaling System #7 (SS7) protocol between telephone network switching centers and other network nodes owned by network operators.
* The main concepts (functional view) surrounding IN services or architecture are connected with SS7 architecture.
* Distribuert system for tjenestestyring.
* IN has not replaced the existing PSTN; rather, it has been overlaid onto it.
* The standards define a complete architecture including the architectural view, state machines, physical implementation and protocols. 
* Before IN was developed, all new feature and/or services that were to be added had to be implemented directly in the core switch systems. This made for very long release cycles as the bug hunting and testing had to be extensive and thorough to prevent the network from failing. With the advent of IN, most of these services were moved out of the core switch systems and into self serving nodes (IN), thus creating a modular and more secure network that allowed the services providers themselves to develop variations and value-added services to their network without submitting a request to the core switch manufacturer and wait for the long development process.
* IN kan grovt sett sees som en tjenesteplattform som "utvider" transportnettet.
* The IN’s main advantage is the ability to control switching and service execution from a small set of Intelligent Network nodes known as Service Control Points (SCP). SCPs are connected to the network switches (known as Service Switching Points (SSP)) via a standardized interface; Signalling System No. 7.
* The SSPs detect when the SCP should handle a service. The SSP forwards a standardized SS7 (TCAP) message containing relevant service information. Via the TCAP message, the service control logic in the SCP directs the SSPs to perform the individual functions that collectively constitute the service.
* An Intelligent Network is able to separate the specification, creation, and control of telephony services from physical switching networks.

### Konseptuell modell

Konseptuell modell i fire nivåer/plan. Each plane introduces an abstract view of the network entities, which is further made tangible in the plane below it. Tolkes top/down:

1. Service plane. Abstrakt spesifikasjonslag. Tjenester beskrives ved hjelp av generelle blokker kalt Service Features (SF). En tjeneste er et selvstendig kommersielt tilbud, og en tjenesteegenskap (SF) beskriver et spesielt aspekt ved en tjeneste. Tjeneste og funksjoner realiseres ved hjelp av funksjoner fra nest øverste plan, såkalte Service Independent Blocks (SIB).
2. Global functional plane. 
   * Modellerer et IN-nettverk som én enkel entitet. 
   * Tjenester identifisert i Service Plane er dekomponert til SF-er, og så mappet på en eller flere SIB-er. 
   * Global tjenestelogikk som styrer instanser av SIB-er. Samlingen av disse representerer et bibliotek som kan anvendes til å realisere tjenester. Tjenestelogikken spesifiserer hvordan et utvalg av slik funksjonalitet skal manøvreres for å skape en tjeneste. 
   * The SIBs are reusable components that can be chained together to construct a service logic. En enkelt SIB er en prosess som kan motta data, prosessere denne, og overlevere til neste trinn i i kjeden. 
   * Basic Call Processing (BCP) er et særtilfelle av en SIB. Utførelsen av en tjeneste starter og ender i BCP. Alltid en Point of Initiation (POI), mens det kan være flere Point of Return (POR). MEN: kun én POR per utførelse. Tenk if-else.
3. Distributed functional plane. Inneholder en beskrivelse av hvordan SIB-funksjonalitet skapes ved hjelp av Functional Entities (FE). FE-kan distribueres i nettet, men én enkelt FE kan ikke distribueres. Altså kan FE-ene til én SIB være distribuert. Hver FE må utføre en Functional Entity Action (FEA), som er realisert ved hjelp av Elementary Functions (EF). Informasjonsflyten mellom flere FE-er går via FEA-ene. SIBs are realized by a sequence of FEAs in an FE. For at en gruppe funksjonelle enheter skal kunne realisere en SIB må de ha de nødvendige egenskaper og kunne samarbeide.
4. Physical plane. Protokoller og prosessering som beskriver fordelingen av fysiske enheter i nettet. The FEs of the distributed functional plane are mapped to Physical Entities (PE). PEs communicate with each other by exchanging protocol messages (represented by information flows in the distributed functional plane). Describes the physical architecture alternatives for an IN-structured network in terms of potential physical systems, referred to as physical entities (PE), in a network, and interfaces between these PEs. The physical plane architecture describes how functional architecture map into Physical Entities and interfaces.

![Konseptuell IN-modell](https://github.com/kjbekkelund/ttm4130/raw/master/images/in-conceptual-model.png)

An IN-compliant service is first constructed through an FE called the Service Creation Environment Function (SCEF). This FE contains the programming environment, which includes the SIB that a programmer uses to construct an IN-compliant service. Once the service logic is created and tested, it is sent to another FE, the service management function (SMF). This FE deploys the service logic to the service execution FEs and allows for service customization. 

Be familiar with and be able to describe the mode of operations for the most important IN physical and functional entities, including SSP, STP, SCP, SDP and SMF, and what IP indicates in this context/relation. 
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

![PEs and FEs](https://github.com/kjbekkelund/ttm4130/raw/master/images/pe-and-fe.png)

Disse enhetene er stort sett forbundet ved hjelp av SS7-signalisering.

### Service Switching Function (SSF) or Service Switching Point (SSP)

* Co-located with the telephone exchange itself, and acts as the trigger point for further services to be invoked during a call.
* Implements the Basic Call State Machine (BCSM). 
* As each state is traversed, the exchange encounters Detection Points (DPs) at which the SSP may invoke a query to the SCP to wait for further instructions on how to proceed. This query is usually called a trigger. Trigger criteria are defined by the operator and might include the subscriber calling number or the dialled number. The SSF is responsible for entertaining calls requiring value added services.
* Innføring av nummerportabilitet gjør at man må konsultere enn database ved alle oppringninger, altså må man inn i IN ved hver oppringning.

### Service Control Function (SCF) or Service Control Point (SCP) 

* Separate set of platforms that receive queries from the SSP. 
* The SCP contains service logic which implements the behaviour desired by the operator, i.e., the services. 
* During service logic processing, additional data required to process the call may be obtained from the SDF. 
* The logic on the SCP is created using the SCE.
* Enhet som styrer tjenesten.
* Inneholder tjenestestyringslogikk som avgjør hvordan en innkommende henvendelse skal tolkes, hvilke parametre som trenger å hentes inn gjennom oppslag i SDP, og hvordan den videre gang blir.
* The SSP contains Detection Capability to detect requests for IN services. It also contains capabilities to communicate with other PEs containing SCF, such as SCP, and to respond to instructions from the other PEs.
* In case of external Service Data Point (SDP) the SCF can access data through a signalling network.
* The SDP may be in the same network as the SCP, or in another network.
* SCF styrer prosessering av anropet ved henvendelser med IN-støttede funksjoner og/eller henvendelser fra bruker.

### Service Data Function (SDF) or Service Data Point (SDP) 

* Database that contains additional subscriber data, or other data required to process a call. 
* The SDF may be a separate platform, or is sometimes co-located with the SCP.

### Service Creation Environment (SCE) 

* Development environment used to create the services present on the SCP. 
* Proprietary graphical languages have been used to enable telecom engineers to create services directly. The user can use Graphical Interface to manipulate between different functions to formulate a service.
* Samsnakker med SMP.

### Specialized Resource Function (SRF) or Intelligent Peripheral (IP) 

* This is a node which can connect to both the SSP and the SCP and delivers additional special resources into the call, mostly related to voice data, for example play voice announcements or collect DTMF tones from the user.
* Enhet utstyrt med tonesender/-mottaker, opptaker/avspiller for kunde- og systemgenererte meldinger.
* An SCP or Adjunct can request an SSP to connect a user to a resource located in an IP that is connected to the SSP from which the service request is detected. An SCP or Adjunct can also request the SSP to connect a user to a resource located in an IP that is connected to another SSP.

### Signal Transfer Point (STP)

* Enhet/ruter for signaleringstrafikk. Fordeler signaleringstrafikken mot SCP på en hensiktsmessig måte.

### Adjunct (AD)

* Samme funksjonalitet som SCP, men betjener bare én sentral. Har ofte egen SDP, og er som oftest samlokalisert med en trafikksterk sentral.

### Network Access Point (NAP)

* Sentral uten IN-funksjoner.
* Må benytte seg av SSP-utstyr i andre sentraler for å få tilknytning.
* This NAP cannot communicate with an SCF, but it has the ability to determine when IN processing is required. It must send calls requiring IN processing to an SSP.

### Service Node (SN)

* Separat sammensatt node med en rekke funksjoner, satt sammen for å dekke lokalt behov. Stort sett samme funksjonalitet som en SSP, men ikke tilknyttet en sentral, altså kan den ikke behandle "normale" anrop.

![Nettverksarkitektur IN](https://github.com/kjbekkelund/ttm4130/raw/master/images/in-network-architecture.gif)

![Funksjonelle enheter](https://github.com/kjbekkelund/ttm4130/raw/master/images/in-distributed-functional-plane-model.png)

Know what Signalling System No 7 is and how it is used
------------------------------------------------------

* SS7 er et felleskanalsystem
* Signaling System Number 7 (SS7) is a set of telephony signaling **protocols** which are used to set up most of the world's public switched telephone network telephone calls. 
* The main purpose is to set up and tear down telephone calls. Other uses include number translation, prepaid billing mechanisms, short message service (SMS), and a variety of other mass market services.
* SS7 skaper språket og kommunikasjonssystemet for å overføre meldinger, men det trengs enheter i nettet som tolker disse meldingene og reagerer på hensiktsmessig måte.
* The term signalling, when used in telephony, refers to the exchange of control information associated with the establishment of a telephone call on a telecommunications circuit.
* To typer signalering:
  1. Channel Associated Signaling. With CAS signaling, this routing information is encoded and transmitted in the same channel as the payload itself.
  2. Common Channel Signaling. With CCS signaling information (control information) is transmitted on a separate channel to the data, and, more specifically, where that signaling channel controls multiple data channels.
* The path and facility used by the signalling is separate and distinct from the telecommunications channels that will ultimately carry the telephone conversation.
* SS7, being a high-speed and high-performance packet-based communications protocol, can communicate significant amounts of information when setting up a call, during the call, and at the end of the call. This permits rich call-related services to be developed.
* Due to its richness and the need for a completely separate signalling network for its operation, SS7 signalling is mostly used for signalling between telephone switches and not for signalling between local exchanges and Customer Premise Equipment (CPE).
* SS7 clearly splits the signaling planes and voice circuits. An SS7 network has to be made up of SS7-capable equipment from end to end in order to provide its full functionality.
* The SS7 protocol stack borrows partially from the OSI Model of a packetized digital protocol stack. OSI layers 1 to 3 are provided by the Message Transfer Part (MTP) and the Signalling Connection Control Part (SCCP) of the SS7 protocol. Currently there are no protocol components that provide OSI layers 4 through 6. The User Part provides layer 7.
* SS7 skreddersyr rutiner helt fra fysisk lag og opp.
* To deler:
  * Message transfer part. Beskriver virkemåte for fysisk lag, linklag og nettverkslag. MTP sørger for at informasjonen fra UP blir transportert pålitelig på tvers av SS7-nettet. Connectionless. For tredje generasjons mobilnett erstattes MTP med IPv6, og lager i den forbindelse Stream Control Transport Protocol (SCTP), siden hverken TCP eller UDP har nok støttefunksjoner.
  * User part. Består av ISDN User Part, Mobile User Part, Telephone User Part, Signaling Connection Control Part og Transaction Capabilities Application Part. Er brukere av MTP.
* SS7 kan brukes til å sammenbinde mange slags kommunikasjonssystemer.
* Stream Control Transmission Protocol (SCTP) is a Transport Layer protocol, serving in a similar role as the popular protocols Transmission Control Protocol (TCP) and User Datagram Protocol (UDP). Indeed, it provides some of the same service features of both, ensuring reliable, in-sequence transport of messages with congestion control.
* The Message Transfer Part (MTP) is divided into three levels:
  1. Equivalent to the OSI Physical Layer. Defines the physical, electrical, and functional characteristics of the digital signaling link.
  2. Equivalent to the OSI Data Link Layer. Ensures accurate end-to-end transmission of a message across a signaling link. Implements flow control, message sequence validation, and error checking.
  3. Equivalent to the OSI Network Layer. Provides message routing between signaling points in the SS7 network. Re-routes traffic away from failed links and signaling points and controls traffic when congestion occurs. 
* ISDN User Part (ISUP). Defines the protocol used to set-up, manage, and release trunk circuits that carry voice and data between terminating line exchanges.
* Signaling Connection Control Part (SCCP). Provides connectionless and connection-oriented network services.
* Transaction Capabilities Applications Part (TCAP). Supports the exchange of non-circuit related data between applications across the SS7 network using the SCCP connectionless service. Queries and responses sent between SSPs and SCPs are carried in TCAP messages.

![SS7 arkitektur](https://github.com/kjbekkelund/ttm4130/raw/master/images/ss7-architecture.png)
![Comparison of transport layers](https://github.com/kjbekkelund/ttm4130/raw/master/images/comparison-transport-layer.png)

### Signaleringssystemer i IP-nett

* SIP. Basis for call control in 3GPP.
* H.323. Multimedia communication over LAN.

Know what TMN is and what it is used for
----------------------------------------

TMN = Telecommunication Management Network.

* Elektronisk drifts- og overvåkningssystem.
* Separat administrativt system. Logisk adskilt fra de systemer som blir administrert.
* Benytter vanligvis separate datalinjer/linker fram til det enkelte nett-element.
* Definerer MIB (Management Information Base)-elementer for hver nettverksenhet som skal styres.
* En del av administrasjonsplanet. Lag:
  * Forretningsdrift
  * Tjenester
  * Nett
  * Nettelementer

Know the meaning of Terminal Mobility, Person Mobility and Service Mobility. Describing mobility handling in the GSM- and IP-world included. 
--------------------------------------------------------------------------------------------------------------------------------------------

Mobility Management is one of the major functions of a GSM or a UMTS network that allows mobile phones to work. The aim of mobility management is to track where the subscribers are, so that calls, SMS and other mobile phone services can be delivered to them.

* Kontinuerlig mobilitet: Avbruddsfri tilgang på tjenester selv under flytting/bevegelse.
* Diskret mobilitet: Tjeneste avbrytes under flytting, men taes opp igjen etterpå. Kan f.eks. har kontinuerlig mobilitet innenfor et gitt dekningsområde, og diskret mobilitet mellom dekningsområder.
* Mobilitet medfører at man tar meg seg /flytter ressurser eller en forbindelse fra et sted i nettet til et annet.  Har da behov for entydig identifisering av terminalen i det nye nettet. I tillegg trenger man autorisasjon for å ta i bruk ressurser i det nye nettet. Og det trengs en rengskapsfunksjon som grunnlag for betaling. => AAA (Authentication, Authorization, Accouting)

Mobilitet handler om å være mobil, ikke trådløs.

<table>
  <tr>
    <th></th>
    <th>LN</th>
    <th>NGN</th>
  </tr>
  <tr>
    <th>Terminal</th>
    <td>Tjeneste opprettholdes, terminal skifter posisjon.</td>
    <td>1) Kunnne fysisk flytte en terminal. 2) Mulighet til å aksessere telekom-tjenester fra forskjellige lokasjoner og i bevegelse.</td>
  </tr>
  <tr>
    <th>Tjeneste</th>
    <td>Programvare kan flyttes fra en maskin til en annen.</td>
    <td>Kunne bruke en tjeneste uavhengig av brukerens og terminalens lokasjon.</td>
  </tr>
  <tr>
    <th>Bruker</th>
    <td>Tjenestetilgang uavhengig av spesifikk terminal.</td>
    <td>Mulighet for abonnent å bevege seg til andre lokasjon og kunne bruker 1->n enheter til å koble på 1->n aksessnettverk for å få tilgang til tjenester.</td>
  </tr>
  <tr>
    <th>Person</th>
    <td>Benytte tjenester tilpasset egne preferanser og brukere uavhengig av fysisk lokasjon.</td>
    <td>Muligheten for en bruker å aksessere alle telekom-tjenester fra enhver terminal på bakgrunn av personlig identifikasjon.</td>
  </tr>
  <tr>
    <th>Rolle</th>
    <td>Flere roller/profiler. "Jeg vil være en person på jobb og en annen hjemme."</td>
    <td></td>
  <tr>
    <th>Sesjonskontinuitet</th>
    <td>Bytte terminal, beholde sesjon (Sesjonsmobilitet)</td>
    <td>Endre nettverksaksesspunkt mens man opprettholder en sesjon. Gjelder terminal og bruker.</td>
  </tr>
  <tr>
    <th>Nomadism</th>
    <td></td>
    <td>Endre nettverksaksesspunkt ved bevegelse. "Hardt" bytte. Diskret.</td>
  </tr>
  <tr>
    <th>Hand-over</th>
    <td></td>
    <td>Special case of session continuity where the incurred interruption or loss of data is below certain limits such that real-time services can be continued despite of the change of access point</td>
  </tr>
</table>

### Mobilitetshåndtering i GSM

* Kan sees på som terminalmobilitet, men SIM-kortet kan flyttes til en ny terminal. => Brukermobilitet. SIM-kortets identitet er referansen.
* Home Location Register (HLR, abonnentregister) inneholder informasjon om tjenester det kan abonneres på, abonnentets status, og oppdatert oppholdsplass for abonnenten, altså hvilket Visiting Location Register (VLR, oppholdsregister) bruker er i.
* Roaming = En bruker kan bevege seg på tvers av operatørområder. Roaming is defined as the ability for a cellular customer to automatically make and receive voice calls, send and receive data, or access other services, including home data services, when travelling outside the geographical coverage area of the home network, by means of using a visited network. Må autentiseres og autoriseres i besøksområdet.
* Terminalen scanner kontinuerlig området den befinner seg i, og rapporterer signalstyrke og andre data til BSC (Base Station Controller), som tar avgjørelsen om handover. Hard handover.
* Anrop gjøres alltid via HLR => Rutes via hjemmeområdet.
* Handover: 
  * Kontinuerlig tjeneste selv om brukeren beveger seg fra et dekningsområde til et annet.
  * Kriterie er i hovedsak kvalitet på mottatt signal. I GSM er det BSC (Base Station Controller) som tar bestemmelsen om handover.
  * Hard handover.

![Location updating procedure in GSM](https://github.com/kjbekkelund/ttm4130/raw/master/images/location-update-gsm.png)
![Anrop til GSM-mottaker som ikke er i hjemmeområdet](https://github.com/kjbekkelund/ttm4130/raw/master/images/gsm-visiting-location.png)

SIM inneholder:

* Korttype
* Serienummer
* Tjenestetabell
* IMSI
* PIN og PUK
* Autentiseringsnøkkel k<sub>i</sub>

### Mobilitetshåndtering i IP

* Ligner GSM, men det heter Home Agent (HA) og Foreign Agent (FA) istedetfor hhv HLR og VLR. HA og FA er samplassert med lokale rutere.
* Nomade betegner en mobil vertsmaskin.
* Korrespondent brukes som betegnelse på en vertsmaskin som kommuniserer med nomaden.
* Dersom nomaden flyttes til et nytt område (et annet subnett), må den først registrere seg for FA. Adressen til FA finnes ved å lytte til Agent Announcements. Dersom det finnes en egnet FA, kan registreringen finne sted. FA oppdaterer HA om ny posisjon. Trafikk vil kapsles inn av HA, som sender det videre til FA, som pakker ut datagrammet og presenterer det i eget subnett med nomadens fast IP-adresse som destinasjon (tunnelering).
* Registrering hos FA har som regel en endelig varighet, og må fornyes, og dette er nomadens plikt å ha oversikt over.
* I IPv4 vil all trafikk til nomaden alltid rutes via HA.

#### IPv6

* Tilstandsløs tilordning: Tillater en nomade å selv finne seg en IP "care of"-adresse på et nytt sted. De siste 48 bit i en autokonfigurert adresse vil være lik ethernet-adressen til vertsmaskinen. Dette betyr at også Network Point of Attachment (NPA) er kjent. Vertsmaskinen kan så melde sin "care of"-adresse til egen HA gjennom en Binding Update-melding. Den virker da som sin egen FA.
* Ruteoptimalisering gir korrespondent mulighet til å cache nomadens c/o-adresse, og triangulær ruting kan da unngås slik at all trafikk ikke lengre trenger å rutes via HA.
* Både Nomade og Korrespondent benytter hjemmeadresse over IP-laget, mens data sendes ved bruk av "care of"-adresse.

![Ruteoptimalisering i IPv6](https://github.com/kjbekkelund/ttm4130/raw/master/images/ruteoptimalisering-mobil-ipv6.png)

### Mobilitetshåndtering i SIP

* Home Registrar og Visiting Registrar istedenfor hhv HLR og VLR i GSM.
* Roaming
  * Finne SIP-tjener via multicast `REGISTER`
  * Kan få IP-adresse via DHCP
  * Oppdaterer hjemmeserveren
* Sesjonsmobilitet: Nomade oppnår ny IP-adresse (eks via DHCP), og sender ny `INVITE` eller `RE-INVITE` til korrespondent, med ny adresse og oppdatert sesjonsbeskrivelse. Trenger først å oppdatere HR når man ønsker å motta nye henvendelser.

Foreslått system:

* Cell handoff. Ruting av datagrammer skiftes.
* Subnett handoff. Mobil får tildelt ny IP-adresse, og sender ny `INVITE` eller `RE-INVITE` til korrespondent. For å ikke tape data, opprettes en kortvarig tunnel.
* Roaming. Subnett handoff + autentisering + autorisering. Kan gjøres uavhengig av underliggende nett-teknologi.

Be able to describe what AAA means and where these functions are used. To be able to give a simple explanation/characterization of the protocols Radius and Diameter 
--------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Autentisering, autorisering, Regnskapsføring (Accounting)**

* Disse funksjonene blir viktigere etterhvert som den enkelte terminal og bruker tillates å operere i mange forskjellige nett, samt bruke et rikt tjenesteutvalg supplert fra mange leverandører.
* Autentisering og autorisering kreves ofte flere ganger, f.eks. for å bli registrert som bruker/abonnement, for å få nettadgang, og for å bruke et bestemt tjenesteutvalg.
* Vertikal -> horisontal integrasjon. Stor betydning med riktig utforming av AAA-funksjonalitet.
* To viktige fora for fastlegging: 3GPP som spesifiserer fremtidens mobilnett, og IETFS arbeidsgrupper for AA og Mobile IP.
* Terminologi:
  * Adminstrativt område: Samling av nettverk, datamaskinter og databaser under en felles administrasjon.
  * Betjening: Funksjonalitet som realiserer tjenestegrensesnittet mellom klienten og lokaldomenet.
  * Portvakt: Enhet som styrer/kontrollerer klientens adgang til nettet.
  * Avregning/Billing: Sette sammen og utstede faktura/regning.
  * Megler: Tiltrodd mellommann med tillit fra minst to andre AAA-tjenere, og som er i stand til å oppnå og yte sikringstjenester fra disse AAA-tjenerne.
  * Fremmeddomene: Administrativt domene besøkt av mobil IP-klient. 
  * Regnskapsføring/Accounting: Funksjonen å samle sammen informasjon om ressursbruk for senere bruk i trendanalyser, revisjon, fakturering eller fordeling av kostnader.
  * CDR (Charging Data Record): Kostnadsregistrering generert av nettelement for å gi underlag for regning.

### Authentication

* Fysisk adgang. Tradisjonell fasttelefoni.
* Terminalidentitet. NMT.
* Passord/PIN-kode
* Biometrisk data
* Flyttbart identitetskort. Brukes i GSM (SIM) og UMTS (USIM).
* Mutual auth veldig viktig. Eks: Det er ikke nok at en basestasjon kan autentisere en mobil, dersom basestasjonen er falsk. Mobilen må også kunne autentisere basestasjonen.

### Authorization

### Accounting

* Viktig element for å overvåke normal drift.
* Ideelt sett skal et system som medfører betaling for kunden også kunne produsere nøyaktig faktureringsgrunnlag. Kan kreve identifikasjon i flere trinn, f.eks. ved adgang til tilleggstjenester.
* Innsamlet statistikk kan danne grunnlag for konfigurasjon, utbygging, osv.

### Basismodell

![AAA-tjenere i hjemme- og lokaldomenet.](https://github.com/kjbekkelund/ttm4130/raw/master/images/aaa-local-home-domain.png)

* Betjeningsnoden (NAS/FA/B) har ofte ikke direkte adgang til data som trengs for å sluttføre transaksjonen. Konsulterer da en autoritet, som vanligvis ligger i det samme administrative domenet, for å skaffe seg bevis for at klienten har akseptable fullmakter.
* Siden betjeningsnoden og den lokale autoritet tilhører samme administrative domene antas det at de har etablert, eller er i stand til å etablere, en sikker kanal som tillater utveksling av informasjon relatert til adgangskontroll.
* Når lokal autoritet ikke har nok informasjon til å verifisere klientens fullmakter forutsettes det at AAAL kan forhandle seg fram til verifikasjon ved hjelp av eksterne autoriteter.
* Det kan være mange betjeningsnoder i hvert administrative domene
* Enhver lokal betjeningsnode bør ha en sikkerhetsrelasjon med den lokale AAA-tjener (AAAL)
* Lokal autoritet må dele, eller dynamisk etablere, sikkerhetsrelasjoner med eksterne autoriteter som er i stand til å sjekke klientfullmakter.
* Betjeningsnoden må kunne holde rede på tilstand for klientforespørsler mens lokal autoritet kontakter passende ekstern autoritet.
* Siden den mobile node ikke nødvendigvis starter sin karriere med å være tilstede i eget hjemmedomene må den være i stand til å bringe tilveie fullstendinge og ufalskbare fullmakter uten noen gang å ha vært i kontakt med hjemmedomenet.
* Siden den mobile nodes fullmakter må holdes ufalskbare må ikke mellomliggende noder være i stand til å lære noe som setter dem i stand til å rekonstruere og gjenbruke fullmakter.
* En betjeningsnode må kunne håndtere forespørsler fra mange klienter samtidig.
* Betjeningsnoden må være beskyttet mot replay attacks.
* Digitale sertifikater bør kunne fraktes i en AAA-melding, og AAA-infrastruktur bør også kunne assistere med hensyn på validering av fullmakter, slik at fremmedagenten og hjemmeagenten blir avlastet.
* Støtte for regnskapsføring.
* Krav relatert til IP-operasjon:
  * Enhver AAA-tjener må være i stand til å tilordne passende IP-adresser på forespørsel fra klient.
  * AAA-tjener må være i stand til å identifisere klienten også ved hjelp av andre hjelpemidler enn IP-adresse.
* Mobil IP(v4):
  * Betjeningsnode = Fremmedagent.
  * AAAH og HA trenger ikke tilhøre samme domene, eksempelvis kan AAAH-tjeneren tilhøre et spesialdomene for et kredittkortfirma.
  * For mobil IP får AAAH og AAAL følgende tilleggsoppgaver:
    * Reautentisering av mobile enheter
    * Autorisasjon av mobil node
    * Initiering av regnskapsfunksjoner for å registrere bruk av tjenester
    * Bruk av AAA-protokoll extensions for å inkludere Mobile IP registreringsmelding som del av den første registreringssekvens.

((Se nærmere på IPv6, s 144 ->))

### Radius

Radius = Remote Authentication Dial In User Service

* Networking protocol that provides centralized AAA management for computers to connect and use a network service.
* En mellomliggende node, f.eks. en Network Access Server virker som en Radius-klient, og tar kontakt med en Radius-tjener. NAS vurderer brukeren, initierer punkt-til-punkt-protokollen, og forhandler med bruker om en IP-adresse. De brukerne som NAS betjener utgjør et eget IP subnett.
* Client/server protocol that runs in the application layer, using UDP as transport
* RADIUS serves three functions:
  1. to authenticate users or devices before granting them access to a network,
  2. to authorize those users or devices for certain network services and
  3. to account for usage of those services.

![RADIUS](https://github.com/kjbekkelund/ttm4130/raw/master/images/radius.png)

### Diameter

I Diameter er det definert et sett av grunnfunksjoner, Diameter Base, som ikke er tiltenkt brukt alene, men sammen med appliasjonsspesifikke tillegg, eks:

* Extensible Authentication Protocol
* Credit-Control
* Network Access Server
* Mobile IPv4
* Command Codes for the 3GPP Release 5

I Diameter kan man enten benytte hele AAA-spektret eller kun Accounting. 

Sentrale forskjeller mellom Diameter og Radius:

* Reliable transport protocols (TCP or SCTP, not UDP)
* Network or transport level security (IPsec or TLS)
* Larger address space for attribute-value pairs (AVPs) and identifiers (32 bits instead of 8 bits)
* Both stateful and stateless models can be used
* Dynamic discovery of peers (using DNS SRV and NAPTR)
* Capability negotiation
* Better roaming support
* More easily extended; new commands and attributes can be defined
* Aligned on 32-bit boundaries
* Basic support for user-sessions and accounting

Basisprotokollen realiserer overføring av AVP-er (attributt-verdi-par). Nye AVP-er kan bli lagt til Diameter-meldinger, så lenge alle de nødvendige kommer med. Man kan altså utvide protokollen.

* En Diameter-kllient er en enhet som utfører adgangskontroll i kanten av nettet. Eks. Network Access Server og Foreign Agent. Genererer Diameter-meldinger for å kreve autentisering, autorisering eller regnskapsføring på vegne av brukeren.
* En Diameter-agent er en node som ikke autentiserer og/eller autoriserer meldinger lokalt.
* En Diameter-tjener utfører autentisering og/eller autorisasjon av bruker.
* En gitt Diameter-node kan virke som agent for noen og tjener for andre.

Diameters agents are introduced to bring flexibility into the architecture; main advantages:

* distribute administration of systems to a configurable grouping, including the maintenance of security associations.
* used for concentration of requests from a number of co-located or distributed NAS equipment sets to a set of like user groups.
* value-added processing to the requests or responses.
* used for load balancing. A complex network will have multiple authentication sources; agents can sort requests and forward these towards the correct target.

![Mobilitet i IPv4](https://github.com/kjbekkelund/ttm4130/raw/master/images/inter-realm-mobility.png)

Be able to describe what a Meta Protocol is and how the use of such protocols are intended (with reference to ETSIs TIPHON standards and consecutive standards). 
----------------------------------------------------------------------------------------------------------------------------------------------------------------

* Metaprotokoll: Et domeneuavhengig protokollrammeverk. En metaprotokoll beskriver meldinger som skal sendes, deres informasjonsinnhold og oppførselen til systemene — når meldinger skal sendes og når de skal mottas. En slik metaprotokoll er ikke designet for realisering direkte, men tjener som en referanse for andre protokoller. Realiseringer vil da lett kunne samarbeide, siden de er bygd på samme metaprotokoll.
* En metaprotokoll håndteres kompleksiteten ved multiprotokoll samvirke.
* Det er ikke alltid mulig å anvende en metaprotokoll for å generere en fullstending avbildning over til en gitt protokoll. Dette resulterer i at man må 
definere utvidelser til den eksisterende metaprotokollen eller mangler ved den valgte protokoll.
* Metaprotokollen kan bli implementert fullstending i den hensikt å utvikle applikasjonstjenere, eller den kan bli brukt som et verktøy for å utvide eksisterende protokoller og sørge for samvirke mellom dem.
* TIPHON-metaprotokollen definerer en funksjonalitet på applikasjonslagesnivå som omfatter en mengde applikasjoner som anses som nøvendige i neste generasjons telefoni. Metaprotokollen består av tilstandsmaskiner som kan utføre anropsbehandling/sesjonsstyring.
* Grunnlaget for metaprotokollen er den abstrakte arkitekturen i TISPAN, som definerer funksjonelle lag, referansepunkt og grensesnitt mellom funksjonelle lag. Et funksjonelt lag utfører spesielle sett av oppgaver.
* Ved å mappe en meta-protokoll til en spesifikk nettløsning, kan man sikre en effektiv veg for å realisere sammenkobling ende til ende.
* Ved å anvende denne tilnærmingen får man en arkitektur for pakkesvitsjet telefoni som kan anvendes for et tjenestetilbud tilsvarende det man har i tradisjonelle telefonnett, og som kan tilby en utviklingsvei over mot bredbåndsapplikasjoner så vel som mobile anvendelser.
* Overgang fra metaprotokoll-beskrivelse til en konkret protokoll skal skje via en ETSI-spesifisert avbildningsfunksjon (mapping function).
* Metaprotokollene etablerer et felles språk som kan spenne over flere varianter av teknologi og underliggende løsninger.

![Samvirke definert ved hjelp av metaprotokoll](https://github.com/kjbekkelund/ttm4130/raw/master/images/meta-protocol.png)

Be able to give a review on problems concerning interworking between traditional and IP-based telecommunication networks
------------------------------------------------------------------------------------------------------------------------

The main issues are:

1. Addressing
2. Signaling 
3. Media conversion
4. Which gateway to use where

History:

* Past: PINT/SPIRITS (signalling)
* Now: Reference model/gateways (signalling + media)
* Future: NGN (TIPHON/TISPAN)

Hovedproblemstillingen er samdrift mellom et IP-nett og CSN. Men også mange andre problemstillinger. Blant annet ønsker man å beskytte tidligere investeringer, og kunne ha en gradvis eller trinnvis overgang fra GSM til UMTS, og en gradvis overgang fra en infrastruktur basert på linjesvitsjing til en infrastruktur basert på pakkesvitsjing.

To former for samdrift:
* Samdrift av styringssystem, altså samvirke mellom IN-basert styringslogikk og internettbaserte styringssystemer.
* Samdrift av styringssystem og samband, altså sammenkobling av IP-baserte og CS-baserte samtaleveier.

### Samdrift av styringssystem

* IETF tok opp problemet med samvirke mellom IN-tjenester i det offentlige telefonnetter (PSTN) og IP-nettet i arbeidsgruppen PINT (PSTN/Internet Interfaces).
* PINT adresser "anrop" eller initiering fra internettsiden mot PSTN. Formålet er i hovedsak å kunne tilby telefontjenester initiert ved klikk på en webside. (Klikk for å ringe opp, klikk for å fakse, klikk for å få faks tilbake, taletilgang til webtjenester).
* Arbeid videreført i SPIRITS (Service in the PSTN/IN Requesting Internet Service), som ser på initiering fra PSTN mot internett.
* Komplekst system. Startet derfor med arbeidet med å få til en full samdrift mellom IP-telefoni og PSTN => Gateways

![PINT referansemodell](https://github.com/kjbekkelund/ttm4130/raw/master/images/pint-reference.png)

### Samdrift av styringssystem og samband

* Neste generasjons nett (NGN) er forutsatt å være et IP-basert tjenesteintegrert nett, med mekanismer for differensiert styring av tjenestekvalitet.
* 5 scenarier for samvirke:
  1. IP-nett til IP-nett (ikke samvirke, ren NGN)
  2. IP-nett til CSN
  3. CSN til IP-nett
  4. CSN til CSN via IP-nett
  5. IP-nett til IP-nett via CSN
* For de scenarier en starter i ett type nett og ender i en annen må man
  * konvertere adresseformater (eks IP -> E.164).
  * konvertere mellom forskjellige signaleringsprotokoller/-systemer.
  * konvertere mellom forskjellige transportprotokoller (eks. linjesvitsjing til TCP/IP).
  * konvertere fra en type kildekoding til en annen.
* Må finne ut hvor eventuelle overgangsenheter (Interworking Function, IWF) er plassert.
* Ang scenariene:
  * (2) Det kan det rent økonomisk være lurt å gå over så sent som mulig, men eventuell dårlig tjenestekvalitet i nettet kan betinge en tidligere overgang.
  * (4,5) åpner for løsning der man oppretter en tunnet gjennom transittnettet. Dersom man skal tunnelere IP-telefoni gjennom PSTN/CSN må man passe på å sikre seg mot transkoding underveis, siden flere nett komprimerer talesignaler for å øke antall tilgjengelige samband på en strekning. Man må ha en ren digital, linjesvitsjet forbindelse.
  * (3) Hvordan oversette E.164-nummer til IP-adresser?
  * (3) Hvordan betjene nomadiske IP-brukere?
  * (4) IP-strekningen introduserer en økt og variabel tidsforsinkelse.
  * (2-5) Oppsetning av rene to-parts telefoniforbindelser kan løses relativt enkelt, men multimedie-sesjoner på tvers av CS- og PS-domenet kan by på større problemer.

![Internet/PSTN gateway]((https://github.com/kjbekkelund/ttm4130/raw/master/images/internet-pstn-gateway.png)

### IMS

* It is desirable for the IMS to interwork with legacy CS networks. This requires interworking both at the user plane and the control plane. 
* Control plane interworking is tasked to the MGCF (Media Gateway Control Function). It performs mapping from SIP signalling to the BICC or ISUP used in CS legacy networks.
* The IMS-MGW (Media Gateway) translates protocols at the user plane. It terminates the bearer channels from the CS networks as well as media streams from IP or ATM.

**IMS -> CS**

An IMS need not bother about whether the called user is an IMS or a CS user. The session request from the calling user will always arrive at the S-CSCF serving the calling user. When the S-CSCF receives a session request using a tel URL type of user identity, it has to perform an ENUM query to convert the tel URL to a SIP URI. If the S-CSCF is able to convert the identity to a SIP URI format it will route the session further to the target IMS network, and when this conversion fails the S-CSCF will try to reach the user in the CS network.

To break out to the CS network, the S-CSCF routes the session request further to the BGCF (Breakout Gateway Control Function) in the same network. The selected BGCF has two options: 

1. Selecting the breakout point in the same network. The BCGF selects an MGCF in the same network in order to convert SIP signalling to ISUP/BICC signalling and control the IMS-MGW.
2. Selecting another network to break out the CS network. Selects another BGCF in a different IMS network to select an MGCF in its network for handling breakout.

![IMS -> CS](https://github.com/kjbekkelund/ttm4130/raw/master/images/ims-to-cs.png)

**CS -> IMS**

When a CS user dials an E.164 number that belongs to an IMS user, it will be handled in the CS network like any other E.164 number; however, after routing analysis it will be sent to an MGCF in the IMS user's home network. After receiving the ISUP/BICC signalling message, the MGCF interacts with the IMS-MGW to create a user-plane connection, converts ISUP/BICC signalling to SIP signalling and sends a SIP INVITE to the I-CSCF, which finds the S-CSCF for the called user with the help of the HSS. Then the S-CSCF takes the necessary action to pass the SIP INVITE to the UE. Thereafter, the MGCF continues communication with the UE and the CS network to set the call up.

![CS -> IMS](https://github.com/kjbekkelund/ttm4130/raw/master/images/cs-to-ims.png)

Be able to account for the most employed addressing and number systems within communication
-------------------------------------------------------------------------------------------

### DNS

* The Domain Name System (DNS) is a hierarchical naming system for computers, services, or any resource participating in the Internet. 
* Translates domain names meaningful to humans into the numerical (binary) identifiers associated with networking equipment for the purpose of locating and addressing these devices world-wide.
* People take advantage of this when they recite meaningful URLs and e-mail addresses without having to know how the machine will actually locate them.
* The Domain Name System distributes the responsibility of assigning domain names and mapping those names to IP addresses by designating authoritative name servers for each domain. Authoritative name servers are assigned to be responsible for their particular domains, and in turn can assign other authoritative name servers for their sub-domains. This mechanism has made the DNS distributed, fault tolerant, and helped avoid the need for a single central register to be continually consulted and updated.
* The domain name space consists of a tree of domain names. Each node or leaf in the tree has zero or more resource records, which hold information associated with the domain name.
* A domain name usually consists of two or more parts (technically labels), which are conventionally written separated by dots.
* The Domain Name System is maintained by a distributed database system, which uses the client-server model. The nodes of this database are the name servers. Each domain or subdomain has one or more authoritative DNS servers that publish information about that domain and the name servers of any domains subordinate to it. The top of the hierarchy is served by the root nameservers.
* The client-side of the DNS is called a DNS resolver. It is responsible for initiating and sequencing the queries that ultimately lead to a full resolution (translation) of the resource sought.
* Resolving usually entails iterating through several name servers to find the needed information. However, some resolvers function simplistically and can communicate only with a single name server. These simple resolvers rely on a recursive query to a recursive name server to perform the work of finding information for them.
* Grunnene til å ikke ha et sentralisert DNS inkluderer: single point of failure, for stort trafikkvolum for én tjener, fjerntliggende sentralisert database, ikke skalerbar.
* Alle vertsmaskiner må ha minst en autorativ navnetjener.
* Because of the huge volume of requests generated by a system like DNS, the designers wished to provide a mechanism to reduce the load on individual DNS servers. To this end, the DNS resolution process allows for caching.

![Iterativt DNS-oppslag](https://github.com/kjbekkelund/ttm4130/raw/master/images/dns-lookup.png)

### IPv6

A subnetwork, or subnet, describes networked computers and devices that have a common, designated IP address routing prefix. Subnetting is used to break the network into smaller more efficient subnets to prevent excessive rates of Ethernet packet collision in a large network. All hosts within a subnet can be reached in one "hop" (time to live = 1), implying that all hosts in a subnet are connected to the same link. A typical subnet is a physical network served by one router.

* Adressefelt på 128 bit
* Tre typer adresser:
  * Unikast. Identifiserer et enkelt grensesnitt, og pakker med en unikast-adresse blir levert til denne adressen. Kan kalles *gitt mottaker*.
  * Anykast. Identifiserer et sett med grensesnitt, vanligvis tilhørende forskjellige noder i nettet. Pakker med anykast-adresse skal leveres til *én* av de aktuelle grensesnittene. An anycast address is a single address assigned to multiple nodes. Delivers to the "nearest" one, according to the routing protocols' measure of distance.
  * Multikast. Identifiserer et sett med grensesnitt, vanligvis tilhørende forskjellige noder i nettet. Pakker med multikast-adresse skal leveres til *alle* aktuelle grensesnitt. Et gitt grensesnitt kan være medlem av et virkårlig antall multikast-grupper. 
* I IPv6 blir adressene tilordnet grensesnitt, ikke noder. 
* Addresses are assigned to interfaces, and a single network host or node can have multiple interfaces, or a single interface with multiple addresses.

![Adressetyper i IPv6](https://github.com/kjbekkelund/ttm4130/raw/master/images/ipv6-address-types.png)

#### Uspesifisert

Must never be assigned to an interface and is to be used only in software before the application has learned its host's source address appropriate for a pending connection.

#### Loopback

Kan anvendes for å sende IP-pakker til seg selv. Skal normalt ikke nå fysisk nettgrensesnitt. Corresponding to 127.0.0.1 in IPv4.

#### Linklokal unikast

Skal aldri bli videresendt til en annen link. Link-Local addresses are designed to be used for addressing on a single link for purposes such as automatic address configuration, neighbor discovery, or when no routers are present. All interfaces are required to have at least one Link-Local unicast address.

<pre>
| 10 bits  |         54 bits         |          64 bits           |
+----------+-------------------------+----------------------------+
|1111111010|           0             |       interface ID         |
+----------+-------------------------+----------------------------+
</pre>

#### Global unikast

Conventional, publicly routable address, just like conventional IPv4 publicly routable addresses. => Alt annet enn det som er spesifisert i tabellen. Anycast addresses are taken from the unicast address spaces.

<pre>
|         n bits         |   m bits  |       128-n-m bits         |
+------------------------+-----------+----------------------------+
| global routing prefix  | subnet ID |       interface ID         |
+------------------------+-----------+----------------------------+
</pre>

#### Anykast

Anycast addresses are allocated from the unicast address space, using any of the defined unicast address formats.

#### Multikast

An IPv6 multicast address is an identifier for a group of interfaces (typically on different nodes).  An interface may belong to any number of multicast groups.
   
<pre>
|   8    |  4 |  4 |                  112 bits                   |
+------ -+----+----+---------------------------------------------+
|11111111|flgs|scop|                  group ID                   |
+--------+----+----+---------------------------------------------+
</pre>

De tre første bit i flags skal settes til 0, men det siste bit-et, T, indikerer om scope er transient. T = 0 indicates a permanently-assigned ("well-known") multicast address, assigned by the Internet Assigned Numbers Authority (IANA). T = 1 indicates a non-permanently-assigned ("transient" or "dynamically" assigned) multicast address.

scop is a 4-bit multicast scope value used to limit the scope of the multicast group.  The values are as follows:

<pre>
0  reserved
1  Interface-Local scope
2  Link-Local scope
3  reserved
4  Admin-Local scope
5  Site-Local scope
6  (unassigned)
7  (unassigned)
8  Organization-Local scope
9  (unassigned)
A  (unassigned)
B  (unassigned)
C  (unassigned)
D  (unassigned)
E  Global scope
F  reserved
</pre>

* Interface-Local scope spans only a single interface on a node and is useful only for loopback transmission of multicast.
* Link-Local multicast scope spans the same topological region as the corresponding unicast scope.
* Admin-Local scope is the smallest scope that must be administratively configured, i.e., not automatically derived from physical connectivity or other, non-multicast-related configuration.
* Site-Local scope is intended to span a single site.
* Organization-Local scope is intended to span multiple sites belonging to a single organization.
* scopes labeled "(unassigned)" are available for administrators to define additional multicast regions.

<pre>
All Nodes Addresses:    FF01:0:0:0:0:0:0:1 (interface-local)
                        FF02:0:0:0:0:0:0:1 (link-local)
All Routers Addresses:  FF01:0:0:0:0:0:0:2 (interface-local)
                        FF02:0:0:0:0:0:0:2 (link-local)
                        FF05:0:0:0:0:0:0:2 (site-local)
Solicited-Node Address: FF02:0:0:0:0:1:FFXX:XXXX
</pre>

Solicited-Node multicast address are computed as a function of a node's unicast and anycast addresses.  A Solicited-Node multicast address is formed by taking the low-order 24 bits of an address (unicast or anycast) and appending those bits to the prefix FF02:0:0:0:0:1:FF00::/104. Denne adresseformen brukes når en node skal "henge seg på" en eller flere multikastgrupper.

A host is required to recognize the following addresses as identifying itself:

* Its required Link-Local address for each interface.
* Any additional Unicast and Anycast addresses that have been configured for the node's interfaces (manually or automatically).
* The loopback address.
* The All-Nodes multicast addresses.
* The Solicited-Node multicast address for each of its unicast and
anycast addresses.
* Multicast addresses of all other groups to which the node
belongs.

A router is required to recognize all addresses that a host is required to recognize, plus the following addresses as identifying itself:

* The Subnet-Router Anycast addresses for all interfaces for which it is configured to act as a router.
* All other Anycast addresses with which the router has been configured.
* The All-Routers multicast addresses.

### Interface Identifiers

Interface identifiers in IPv6 unicast addresses are used to identify interfaces on a link. They are required to be unique within a subnet prefix.

* Links or Nodes with IEEE EUI-64 Identifiers: The only change needed to transform an IEEE EUI-64 identifier to an interface identifier is to invert the "u" (universal/local) bit.
* Links or Nodes with IEEE 802 48-bit MACs: Create an IEEE EUI-64 identifier from an IEEE 48-bit MAC identifier by inserting two octets, with hexadecimal values of 0xFF and 0xFE, in the middle of the 48-bit MAC (between the company_id and vendor-supplied id).

### Telefonnummer

### GSM og UMTS

* IMSI (International Mobile Subscriber Identity). Identifikator som brukes internt, blant annet til identifikasjon av mobilterminal i radiostyringssystemet for å kunne starte registrering i et besøksområde, fastlegge hjemmenettet til en besøkende mobilterminal/bruker, identifisere terminal/bruker når opplysninger angående tjenester for brukeren utveksles mellom nettene, og identifikasjon av terminal/bruker med hensyn på avregning. Mobil landkode (MCC, 3 siffer) + Nettoperatør (MNC, 2 eller 3 siffer) + Abonnentidentitet (MSIN) = max 15 siffer.
* TMSI (Temporary Mobile Subscriber Identity). Temporær identitet tilordnet av VLR eller SGSN (P-TMSI), som må kunne relateres til aktuell UEs eller MS' (mobile station) IMSI.
* TLLI (Temporary Logical Link Identity). Signalling address for communication between the MS (Mobile Station) and the SGSN. 
* MSISDN (Mobile Station International PSTN/ISDN Number). The standard international telephone number used to identify a given subscriber.
* MSRN (Mobile Station Roaming Number). Temporært anropsnummer som er tildelt MS på besøksplassen. MSISDN brukes til å finne HLR, der man får adgang til MSRN, slik at anropet kan rutes til besøkende MSC. 
* MSIDN (Mobile Station International Data Number). 
* Handover Number. Blir brukt til å etablere kobling mellom MSC-er i forbindelse med handover. Samme form som MSRN.
* IMEI (International Mobile Equipment Identifier). Unique identifier allocated to each ME (Mobile Equipment). It consists of a TAC (Type Approval Code, 8 siffers), SNR (Serial Number,  6 siffers) and a Spare Digit.
* LAI (Location Area Identification). Uniquely identifies a LA (Location Area) within any PLMN (Public Land Mobile Network). It is comprised of the MCC (Mobile Country Code, 3 digits), MNC (Mobile Network Code, 2 or 3 digits) and the LAC (Location Area Code, 2 octets).
* RAI (Routing Area Identity). Composed of the LAC (Location Area Code) and the RAC (Routing Area Code). It is used for paging and registration purposes.
* CI (Cell Identity) & CGI (Cell Group Identity). Under en gitt basestasjon kan man bruke en gitt celle. A 16bit identifier in GSM and UMTS. When combined with the LAI (Location Area Identity) or RAI (Routing Area Identity) the result is termed the CGI (Cell Global Identity).
* BSIC (Base Station Identity Code). For at en MS skal kunne holde rede på basestasjonene i nærheten, gis disse sin spesifikke identifikasjon. 

### Internett

* URI = Uniform Resource Identifier. Consists of a string of characters used to identify or name a resource on the Internet. Such identification enables interaction with representations of the resource over a network, typically the World Wide Web, using specific protocols. URIs are defined in schemes specifying a specific syntax and associated protocols.
* URL = Uniform Resource Locator. A type of URI that specifies where an identified resource is available and the mechanism for retrieving it.
* URN = Uniform Resource Name. A URI that uses the urn scheme, and does not imply availability of the identified resource.

A Uniform Resource Name (URN) functions like a person's name, while a Uniform Resource Locator (URL) resembles that person's street address. The URN defines an item's identity, while the URL provides a method for finding it.

NAI is a standard way of identifying users who request access to a network. Globalt navnerom for brukeridentiteter på formen <brukernavn>@<domenenavn>. Kan grovt si at NAI har samme funksjon i Internett som IMSI i UMTS/GSM.

I fremtidige IP-baserte Multimediakjernenett (IMCN) opererer man med tre typer IP-adresser eller identiteter:

1. Privat brukeridentitet. Tilordnes av hjemmeoperatøren og brukes for all håndtering av abonnement og regninger. Vil blant annet bli larget i HSS, og funnet av og mellomlagret i S-CSCF under registrering.
2. Offentlig brukeridentitet. Den gyldige "anropsidentiteten". Skal ha form som en SIP URL eller eventuelt foreligge i "tel:"-URL-formatet.
3. Temporær offentlig brukeridentitet. 

![Forhold mellom private og offentlige brukeridentiteter](https://github.com/kjbekkelund/ttm4130/raw/master/images/nai-user-identities.png)

CSCF-, BGCF- og MGCF-noder skal kunne identifiseres ved hjelp av gyldige SIP URL-er over de grensesnitt som støtter SIP-protokollen.

Know what is meant by resource discovery and be familiar with some examples
---------------------------------------------------------------------------

Ressursavdekking er en funksjon som tillatter terminalen eller brukeren å oppdage eler kartlegge tilgjengelige ressurser. Ethvert system som gir støtte for flyttbare enheter bør/må ha implementert en eller annen metodikk for ressursavdekking, og etterfølgende konfigurering. 

To løsningsmåter:

* Ressursene annonserer sin tilstedeværelse ved kringkasting eller multicast på mediet (med jevne mellomrom).
* Ved å etterspørre ressursene. Enklest kan dette gjøres når ressursene har fått "velkjente" adresser som kan benyttes for å nå dem.

### Neighbour discovery, IPv6

* Nodes (hosts and routers) use Neighbor Discovery to determine the link-layer addresses for neighbors known to reside on attached links and to quickly purge cached values that become invalid.  Hosts also use Neighbor Discovery to find neighboring routers that are willing to forward packets on their behalf.  Finally, nodes use the protocol to actively keep track of which neighbors are reachable and which are not, and to detect changed link-layer addresses.  When a router or the path to a router fails, a host actively searches for functioning alternates.
* Alle noder trenger å kjenne til minst én ruter, som kan formidle pakker til andre maskinter som ikke befinner seg på samme lokalnett.
* Funksjoner:
  * Avdekking av rutere tilknyttet samme link.
  * Avdekking av prefiks som spesifiserer mottakere på samme link.
  * Parameteravdekking, f.eks. MTU (maksimal overføringsenhet målt i oktetter) og grense for antall hopp.
  * Selvkonfigurering av adresse.
  * Linkadresseoppslag. Hvordan bestemme linklagsadresse til en mottaker på samme link når bare IP-adressen er kjent.
  * Bestemmelse av neste hopp.
  * Deteksjon av nabo som ikke kan nåes.
  * Deteksjon av adressedubletter.
  * Omdirigering.
* Meldingstyper:
  * Ruterforespørring. Når et grensesnitt blir aktivert kan en vertsmaskin sende en slik melding for å trigge øyeblikkelig ruterannonsering.
  * Ruterannonsering. Enten perodisk eller på forespørsel. 
  * Naboforespørring. Sendes ut for å få kjennskap til linklagsadressen til en nabo, for å verifisere at naboen fortsatt kan nåes ved hjelp av en cachet linklagsadresse, eller for å sjekke om det eksiterer adressedubletter.
  * Naboannonsering. 
  * Omruting.
* Naboavdekkingsprotokollen har egentlig Ethernet, linklaget, som hovedreferanse. En linklagsadresse er i den sammenhegn en IEEE 802-basert MAC-adresse. Naboavdekking foregår på samme link.
* De fleste meldingene vil bli sendt med en MAC-adresse
* Linklag:
  * Multikast. A link that supports a native mechanism at the link layer for sending packets to all or a subset of all neighbors.
  * Punkt-til-punkt. A link that connects exactly two interfaces. A point-to-point link is assumed to have multicast capability and have a link-local address.
  * Multiaksess. A link to which more than two interfaces can attach, but that does not support a native form of multicast or broadcast.
* Hop Limit = 255. Slik kan vi sikre oss mot at pakken er videresendt av en ruter, siden ruteren dekrementerer verdien med 1 for hver videresending.

#### RDF

(s. 131 ->)

Be able to account for network and conceptual architecture for next generation networks (TISPAN/NGN-ETSI and IMS) 
-----------------------------------------------------------------------------------------------------------------

It is agreed upon to merge the results of the IMS work undertaken by the 3GPP with the NGN work undertaken in TISPAN.

### NGN

* Next Generation Networking (NGN) is a broad term to describe some key architectural evolutions in telecommunication core network and access networks.
* Målet med TISPAN er å komme fram til en generisk NGN-regeransemodell som omfatter sammensmeltingen av fast-nett, mobilnett og interenett-teknologi. En generisk referansemodell definerer en referansearkitektur som er teknologiuavhengig.
* To fremtidige utviklingsscenarier:
  1. Internett-scenarioet
  2. Teleoperatørscenarioet
* Abstrakt arkitektur med tilhørende grensesnitt og referansepunkter. På grunnlag av denne arkitekturen spesifiserer man så oppførsel og samvirke ved hjelp av metaprotokoller.
* "all-IP" is sometimes used to describe the transformation towards NGN
* In the core network, NGN implies a consolidation of several transport networks into one core transport network

The NGN functional architecture is structured according to a service layer (Application plane) and an IP-based transport layer (Transport plane).

Service layer:

* IMS
* PSTN/ISDN Emulation Subsystem
* Other multimedia subsystems and appliations
* Common components, such as charging, user profile management, security mangement, routing data bases (ENUM), ...

![NGN overall architecture](https://github.com/kjbekkelund/ttm4130/raw/master/images/ngn-overall-architecture.png)

The core network of NGN Release 1 is required to be based upon the IMS

The access network is defined as a collection of network entities and interfaces that provides the underlying IP transport connectivity between the device and the NGN core entities.

The NGN will support a variety of terminals end user equipment.

NGN supports the delivery of end-user services through application servers, rather than directly embedding services as capabilities in the control protocols

Third-party service providers and applications are supported through suitable control interfaces.

NGN standardizes service capabilities and not the services themselves.

Major service capabilities 
* Session establishment and control for session based services, including: Real-time conversational services (voice, multi-media, messaging, etc.), Multimedia Telephony.Videotelephony. 
* Presence.
* PSTN/ISDN migration scenarios for both replacement and evolution (emulation and simulation).

![NGN overview](https://github.com/kjbekkelund/ttm4130/raw/master/images/ngn-overview.png)

NGN should enable all possible types of billing arrangements, as well as accounting (between providers). This includes also e-commerce arrangements.

Billing, charging and accounting in NGN will be based on the collection of information from any appropriate entities in the form of Charging Data Records (CDRs).

--

Domener er en samling av fysiske eller funksjonelle enheter styrt av en administrasjon under et sammenfallende sett av kjøreregler og kompatible teknikker. Tre typer: Sluttbruker, tjeneste og transport.

Funksjonelle grupper beskriver funksjoner som er nødvendig for å kunne tilby IP-telefoni på tvers av domener. Funksjonelle grupper i sluttbrukerdomenet:

* Terminalfunksjonell gruppe. Representerer alle IP-telefonifunksjoner i en brukerterminal. Anropende eller mottakende.
* (Terminal-)registreringsfunksjonalitet: Nødvendige funksjoner for å oppnå og vedlikeholde registrering av en brukerterminal.

Funksjonelle grupper i tjenestedomenet:

* Nettverksfunsjonell gruppe. Samling av nødvendige funksjoner for å kunne etablerer en forbindelse mellom to terminaler, mellom en gateway og en terminal, eller mellom to gateways.
* Gateway-funksjonalitet. Inneholder samme funksjonalitet som nettverksfunksjonell gruppe, men i tillegg har den også funksjoner som tillatter opprettelsen av forbindelse mellom linjesvitsjede og pakkesvitsjede nett. 

### IMS

![IMS Architectural Overview](https://github.com/kjbekkelund/ttm4130/raw/master/images/ims-architectural-overview.png)

* The IP Multimedia Subsystem (IMS) is a global, access-independent and standard-based IP connectivity and service control architecture that enables various types of multimedia services to end-users using common Internet-based protocols. 
* The IMS is an architectural framework for delivering Internet Protocol (IP) multimedia services.
* IMS users are able to mix and match a variety of IP-based services in any way they choose during a single communication session.

A user can roam and obtain IP connectivity from the home network. This would allow IMS services when roaming in an area with an IMS network.

![IMS connectivity when user is roaming](https://github.com/kjbekkelund/ttm4130/raw/master/images/ims-connectivity-roaming.png)

The underlying access and transport networks together with the IMS provide end-to-end Quality of Service (QoS). Negotiates capabilities and expressees its QoS requirements during a SIP session setup. Can negotiate: Media types, direction of traffic, media type bit rate, packet size, packet transport frequency, usage of RTP payload for media, bandwidth adaption.

The IMS has its own authentication and authorization mechanisms between the UE and the IMS network in addition to access network procedures. Supports both online and offline charging capabilities (real time vs not real time).

![IMS charging overview](https://github.com/kjbekkelund/ttm4130/raw/master/images/ims-charging.png)

In 2G mobile networks the operator of the visiting network control the service utilization (visiting service control). IMS has chosen to concentrate on home service control, i.e. that the service execution is to be controlled by the home operator.

3GPP is standardizing service capabilities and not the service themselves. This is done to create scalable service platforms and as a means to launch new services rapidly.

The IMS has a layered architecture. Aima at a minimum dependence between layers. Increases the importance of the application layer as services are designed to work independent of the access network and the IMS is equipped to bridge the gap between them.

![IMS layered architecture](https://github.com/kjbekkelund/ttm4130/raw/master/images/ims-layered-architecture.png)

Categories of IMS entities:

1. Session mangement and routing (CSCF)
2. Databases (HSS, SLF)
3. Services (Application server, MRFC, MRFP)
4. Interworking functions (BGCF, MGCF, IMS-MGW, SGW)
5. Support functions (PDF, SEG, THIG)
6. Charging

In IMS the internal functionality of the network entities is not specified in detail. Instead, spesifications describe the reference points between entities and functionalities supported at the reference points.

**Call Session Control Functions**

![Relationship between CSCFs](https://github.com/kjbekkelund/ttm4130/raw/master/images/cscf-relationship.png)

SIP based proxies:

* **P-CSCF** (Proxy). First contact point for users within the IMS. All SIP signalling trafiic from the UE will be sent to the P-CSCF. Tasks: SIP compression, IPSec security association (SA), interation with Policy Decision Function (PDF), and emergency session detection. Tasked to relay session and media-related information to the PDF when an operator wants to apply SBLP (Service-Based Local Policy). Based on the received information the PDF is able to derive authorized IP QoS information what will be passed to the GGSN when the GGSN needs to perform SBLP prior to accepting a secondary PDP context activation. Via the PDF IMS and GPRS can receive and deliver charging information.
* **I-CSCF** (Interrogating). Contact point within an operator's network for all connections destined to a subscriber of that network operator. Tasks: Obtaining the name of the next hop from the HSS; Assigning an S-CSCF based on received capabilities from the HSS; Routing incomming requests further to an assigned S-CSCF or the application server; Providing Topology Hiding Inter-network Gateway functionality.
* **S-CSCF** (Serving). Focal point of the IMS. Responsible for handling registration processes, making routing decisions and maintaining session states, and storing the service profile(s). Downloads authentication data and service profile from the HSS. A service profile is a collection of user-specific information that is permanently stored in the HSS. Able to send accounting-related information to the Online Charging System.

![S-CSCF routing](https://github.com/kjbekkelund/ttm4130/raw/master/images/s-cscf-routing.png)

**Databases**

Two main databases:

* **Home Subscriber Server** (HSS). Main data storage for all subscriber and service-related data. User identities (private and public), registration information, access paramters and service-triggering functions. Also provides user-specific requirements for S-CSCF capabilities. This information is used by the I-CSCF to select the most suitable S-CSCF for a user. Contains the subset of Home Location Register and Authentication Center (HLR/AuC) functionality required by the PS and CS domain. There may be more than one HSS in a home network.
* **Subscription Locator Function** (SLF). Used as a resolution mechanism that enables the I-CSCF, the S-CSCF and the AS to find the address of the HSS that holds the subscriber data for a given user identity when there are multiple HSSs.

**Service functions**

Multimedia Resource Function Controller (MRFC), Multimedia Resource Function Processor (MRFP) and Application Server (AS).

ASs are not pure IMS entities, they are functions on top of IMS. Resides in the user's home network or in a third-party location. Main functions: Possibility to process and impact an incomming SIP session received from the IMS; capability to originate SIP requests; capability to send accounting information to the charging functions. The services are not limited purely to SIP-based services since an operator is able to offer access to services based on CAMEL (Customized Applications for Mobile network Enhanced Logic) Service Environment (CSE) and OSA (Open Service Architecture). 1->n AS per subscriber, 1-> AS per session.

MRFC and MRFP together provide mechanisms for bearer-related services. The MRFC is tasked to handle SIP communication to and from the S-CSCF and to control the MRFP. The MRFP in turn provides user-plane resources that are requested and insctructed by the MRFC; e.g. mixing of incomming media streams, media stream source, media stream processing.

**Interworking functions**

Four interworking function are needed for exchanging signalling and media between IMS and the circuit switched core network:

* Breakout Gateway Control Function (BGCF). To break out the S-CSCF sends a SIP session request to the BGCF, it further chooses where a breakout to the CS domain occurs. If the breakout happens in the same network, the BGCF selects a MGCF to handle the session further, if not the BGCF forwards the session to another BGCF in a selected network.
* Media Gateway Control Function (MGCF). Performs protocol conversion between SIP protocols and the ISDN User Part (ISUP) or the Bearer Independent Call Control (BICC) and sends the converted request via the SGW to the CS CN.
* Signalling Gateway (SGW). Perform signalling conversion (both ways) at the transport level between the IP-based transport of signalling and the SS7 based transport of signalling. Does not interpret application layer messages (BICC, ISUP). 
* IMS Media Gateway (IMS-MGW). Provides the user-plane link between CS CN network and the IMS. Terminates the bearer channels form the CS network and media streams from the backbone network, executes the conversion between these terminations and performs transcoding and signal processing for the user plane when needed.

![Signalling conversion in the SGW](https://github.com/kjbekkelund/ttm4130/raw/master/images/sgw.png)

**GPRS entities**

* **SGSN** (Serving GRPS Support Node). Links the RAN (Radio Access Network) to the packet core network. Responsible for perform both control and traffic-handling functions for the PS domain. Acts as a gateway for user data tunnelling => relays traffic between UE and the GGSN. Generates charging informaton.
* **GGSN** (Gateway GPRS Support Node). Provides interworking with external packet data networks. Prime function is to connect the UE to external data networks, where IP-based applications and services reside (E.g. IMS or Internet).

**IMS Reference points**

![IMS Reference Points](https://github.com/kjbekkelund/ttm4130/raw/master/images/ims-reference-points.png)

* **Gm**. UE -> P-CSCF. Used to transport all SIP signalling between UE and the IMS. Procedure categories: registration, session control, transations.
* **Mw**. Interface between CSCFs. Procedure categories: registration, session control, transations.
* **ISC**. Between CSCF and AS. Procedure categories: Routing the initial SIP request to an AS; AS initiated requested.
* **Cx**. Subscriber and service data is stored in the HSS and need to be utilized by the I-CSCF and S-CSCF when the user registers and receives sessions. The selected protocol is Diameter. Procedure categories: location management, user data handling and user authentication.

![Hish-level IMS session establishment flow](https://github.com/kjbekkelund/ttm4130/raw/master/images/ims-establishment.png)

Private User Identity. NAI (Network Access Identifier). Included in all registrations. The S-CSCF need to obtain and store the Private NAI, but it is not used for routing SIP messages. Valid through the period of subscription. Not possible to modify for the UE. Stored in the HSS, and may be referred to in CDRs.

Public User Identity. SIP URI og Tel-URL. At least one Public User ID must be stored in ISIM. Must be registered before acting as receiving IMS point. Not possible to modify for the UE. Possible to register multiple public user IDs. 

![Relationship between user identities](https://github.com/kjbekkelund/ttm4130/raw/master/images/ims-user-identities.png)

![Sharing a single user identity between multiple devices](https://github.com/kjbekkelund/ttm4130/raw/master/images/ims-identity-devices.png)

Be able to describe the VoIP architecture: SIP (SIP proper)
-----------------------------------------------------------

* Session Initiation Protocol (SIP) er designet for å sette opp sesjoner i Internett.
* SIP brukes primært for styring (signalering) av selve oppsettet, men kan også benyttes for å gjøre endringer i sesjonen mens den pågår.
* Anvendelsområder:
  * Lokalisering av bruker (Aksessnettet hvor brukeren befinner seg)
  * Brukertilgjengelighet
  * Brukerprofil
  * Oppsetting av sesjon
  * Sesjonsadministrasjon
* SIP beskriver selve arkitekturen for signaleringssystemet, men Session Description Protocol (SDP) benyttes for å beskrive innhold i signaleringsmeldingene. SIP frakter altså ikke mediainnhold mellom brukere, men er en signaleringsprotokoll.
* Artikekturen består i hovedsak av fire typer identiteter:
  * SIP brukeragent.
  * SIP proxy-tjener.
  * DNS.
  * Lokasjonstjener.
* Abonnenten får tildelt en identitet, i form av en SIP Uniform Resource Indicator (URI), som har samme form som en email-adresse.
* Alt utstyr som kan bruke IP/UDP kan være SIP brukeragent.
* Registreringsfunksjonen i SIP kan være lagt til en proxy-tjener. Dersom den legges til egen tjener kalles denne Registrar. En registrar vil normalt skrive resultatet av registreringen i en location server, og virker som en slags HLR i GSM.
* SIP is a permanent element of the IP Multimedia Subsystem (IMS) architecture for IP-based streaming multimedia services in cellular systems.

![Nett-elementer som inngår i SIP](https://github.com/kjbekkelund/ttm4130/raw/master/images/sip-entities.png)

All-IP nature opens up competition space and removes investment barriers.

What’s the adaptation chance: running homogenous all-IP networks greatly reduces cost and increases competitiveness.

SIP is a mature all-IP technology in polishing stage which is in wide use with an array of equipment from various vendors.

* Distributed end-2-end design
* Intelligence and states resides in end-devices
* Network maintains almost zero intelligence (except routing) and state (except routing tables).

Results in flexibility, failure recovery, and scalability.

* SIP is text-oriented protocol – easy to extend and debug
* The world is split in administrative domains with DNS names
* Addresses are described using URIs.

Supports locating users, session negotation and changing session state.

SIP is a client-server request-response protocol. Client sends a reuqest to server and awaits reply. Requests can take arbitrarily complex path, and replies follow the same path in reverse direction. 

Usually uses UDP, but TCP is emerging for "bulk" applications and SCTP is rarely used.

![SIP Call OK](https://github.com/kjbekkelund/ttm4130/raw/master/images/sip-call-ok.png)

SIP gives you a globally reachable address. Callees bind their temporary address to the global one using SIP REGISTER 
method. Callers use this address to establish real-time communication with callees.

**ENUM**

* Translates E.164 numbers into URIs
* Utilizes DNS, reversed dot-separated.

![ENUM Call Flow](https://github.com/kjbekkelund/ttm4130/raw/master/images/enum-call-flow.png)

**Header fields**

* To: URI of the request. A tag can be used to distinguish between different UAs.
* From: URI of the sender
* Cseq: A sequence number and a method name (used for matching requests and responses)
* Call-ID: Unique identifier for a SIP message exchange (signaling session)
* Max-forwards: A number that is decremented by one by every proxy. Request is discarded when it reaches zero
* Via: Keeps track of all the proxies used. The responses use the same chain of proxies when returning.

SIP messages can carry any type of body and even multipart bodies using MIME

SIP: Be able to describe the following concepts and their function: Proxy, Redirect Server, Registrar, User Agent, SDP
----------------------------------------------------------------------------------------------------------------------

### User Agent

* Client originates calls, server listens for incomming calls.
* Softphone, hardphone, webphone, messaging clients, PSTN gateways, ...

### Registrar

* Keeps track of users' whereabouts (Like HLR in GSM)
* Accept registration requests from users

### Proxy

* Looks up next hops for requests to served users in location database and forwards the requests there.
* Relays call signaling, i.e. acts as both client and server)
* Operates in a transactional manner, i.e. no session state.
* Transparent to end-devices
* Allows for additional services (call forwarding, AAA, forking, etc.)
* Maintain ventral role in SIP networks. They glue SIP components such as phones, gateways, applications and other domains by implementing som routing logic.
* Key Roles: Security, Services and Routing.
* Steps:
  * Sanity checks on incoming requests
  * Canonization of dialled target to E.164 if applicable
  * Emergency calls
  * Authenticate originator
  * Execute callers services, e.g. anonymization
  * Check request against originator's privileges
  * Look up recipient
  * If found, try to execute services
  * If not found, try to forward to PSTN
* A proxy may fork a request to multiple destinations either in parallel (“reachme everywhere”) or serially (“forward no reply”). 
* SIP Proxies may operate either in stateful or stateless mode.
  * Stateless. Good for heavy-load scenarios. Proxies just receive messages, perform routing logic, send messages out and forget everything. Memory consumption is constant.
  * Stateful. Good for implementing some services. Maintain state during entire transaction. State is used for services such as accounting, forking, forwarding on some evert. State refers to transactions, not entire calls. Not aware of existing calls. 

![Forking proxy](https://github.com/kjbekkelund/ttm4130/raw/master/images/sip-forking-proxy.png)

### Redirect Server

* Redirects callers to other servers

![Redirect server](https://github.com/kjbekkelund/ttm4130/raw/master/images/sip-redirect-server.png)

Registrar, proxy and redirect server are typically part of a single server.

![Proxy and registrar kept separate](https://github.com/kjbekkelund/ttm4130/raw/master/images/sip-separate-proxy.png)

Be able to describe how SIP is intended integrated in 3GPP/UMTS
---------------------------------------------------------------

* 3GPP-konsortiet baserer seg på SIP for oppsetting.
* I 3GPP benyttes SIP som utgangspunkt for å håndtere multimedieanrop.
* CSCF = Call Session Control Function (sesjonskontroll)
* User Equipment (UE) er en SIP brukeragent som betjenes av Proxy-CSCF (P-CSCF), som igjen kommuniserer med Serving-CSCF (S-CSCF). I 3GPP er det S-CSCF som styrer og vedlikeholder et tilstandsbilde for sesjonen. Dette skjer altså i hjemmenettet til abonnenten.
* P-CSCF
  * Er UEs første kontaktpunkt. 
  * Fungerer som en vanlig proxy, og kan videresende forespørsler på vegne av en bruker. 
  * Skal kunne generere CDR-er, vedlikeholde sikker kommunikasjon mellom seg og UE, utføre meldingskompresjon og -dekompresjon, samt å ta seg av autorisering av bærer-ressurser og QoS-administrasjon.
* Interrogating-CSCF (I-CSCF)
  * Kontaktpunkt mot en operatørs nett for alle forbindelser som ankommer til brukere hos denne operatøren, og til gjestebrukere som oppholder seg i området. 
  * En operatør kan ha flere I-CSCF-er. Er en slags grensevakt. 
  * Finner adressen til S-CSCF ved hjelp av Home Subscriber Server (HSS). 
  * Tilordner S-CSCF til en bruker som utfører SIP-registrering. 
  * Ruter SIP-forespørsler mottatt fra andre nett til egnet S-CSCF i eget nett. 
  * Genererer CDR-er.
  * Nettoperatør trenger ikke benytte seg av I-CSCF, men fordelen er at man kan gjemme vekk opplysninger om den indre strukturen i nettet.
* S-CSCF
  * Betjener bruker under sesjonen.
  * The brain of the IMS.
  * Located in the user’s home network
  * Utfører styring av sesjonen på vegne av UE.
  * The S-CSCF performs session control and registration services for the UE. During the session, the S-CSCF maintains the session state and interacts with the service platforms and charging functions as required by the network operator to provide the services. There can be a number of S-CSCFs in an operator’s network and the S-CSCFs can provide a variety of unique functions depending on the nature of the application service platforms. In essence the S-CSCF marries the capabilities of the UE with the services of the application servers.
  * Funksjoner:
    * Handle registration requests
    * Authenticate users
    * Download information from the HSS
    * Route traffic to P-CSCF/I-CSCF/BGCF/AS as needed: The S-CSCF routes mobile-terminating traffic to the P-CSCF. It also routes mobile-originated traffic to the I-CSCF, the breakout gateway control function (BGCF), or the application server (AS).
    * Perform session control: Act as a SIP proxy server and/or user agent
    * Supervise registration
    * Execute media policing
    * Maintain session timers
    * Generere CDR-er
* Breakout Gateway Control Function (BGCF)
  * To move from the IMS to the circuit-switched domain, the IMS uses the breakout gateway control function (BGCF), which determines where the breakout to the circuit-switched domain occurs. 
  * Tilordner Media Gateway Control Function (MGCF) som vil være ansvarlig for samvirke med PSTN/CS Domain.
  * Velger hensiktsmessig sted for overgang til PSTN/CS Domain.
* Basissesjoner mellom to mobile brukere vil alltid benytte to S-CSCF-er.
* En basissesjon mellom en bruker i UMTS-multimedienett og et punkt i PSTN involverer én S-CSCF for UE-en, én BGCF for å velge PSTN-gateway, og én MGCF for styring av mediekonvertering via a vis PSTN.
* S-CSCF tilsvarer CCAF i IN, den er grensen mellom bruker og oppsetning i nettet.

![Omgivelser for SIP-basert tjenestestyring i UMPS IP-basert multimediakjernenett](https://github.com/kjbekkelund/ttm4130/raw/master/images/sip-environment.png)

Be able to describe the underlying idea/purpose of PARLAY/OSA and how it is realized
------------------------------------------------------------------------------------

Parlay/OSA is an API that enables operator and 3rd party applications to make use of network functionality through a set of open, standardised interfaces. The Parlay APIs were designed to be technology-independent, and they work equally well for fixed and mobile networks, and for current and next-generation networks. 

Anvendelse av APIer i et kommunikasjonssystem medfører at det er mulig å skrive flyttbare applikasjons som kan kjøres over en mengde underliggende protokoller uten å måtte endre disse.

Parlay is an open API for the telephone network (fixed and mobile). The objective of Parlay/OSA is to provide an API that is independent of the underlying networking technology and of the programming technology used to create new services. As a result the Parlay/OSA APIs are specified in UML. There are then a set of realizations, or mappings, for specific programming environments: CORBA/IDL, Java, WebService (WSDL).

Parlay/OSA can be used to extend or replace an existing Intelligent Network. The reasons are improved time to market, and enabling the development of value-added services that are network independent. Network independence is important when planning for the transition to IP-based Next Generation Networks since it preserves the investment in existing services. 

Opening up of network by means of standardized APIs based on open technology leads to:

* Shorter TTM for applications/services due to abstraction and open technology
* Applications can be developed and deployed by 3rd parties
* Applications can be network independent

The Parlay/OSA Gateway consists of several Service Capability Servers (SCS): functional entities that provide Parlay/OSA interfaces towards applications. 

Each SCS is seen by applications as one or more Service Capability Features (SCF): abstractions of the functionality offered by the network, accessible via the Parlay/OSA API. Sometimes they are also called services. The Parlay/OSA SCFs are specified in terms of interface classes and their methods.

SCSs provide the applications with SFCs, and are abstractions from underlying network functionality. From the viewpoint of the applications, an SCS can be seen as a resource to the core network.

Services using Parlay are meant to reside outside the network trust boundries.

![Parlay/OSA](https://github.com/kjbekkelund/ttm4130/raw/master/images/parlay-osa.png)

The OSA Gateway is viewed by the Applications as a set of interfaces, grouped in terms of functionality into discoverable Service Capability Features (SCF), of two kinds:

* One is a single, always present (one per network) SCS, called the Framework. The OSA Framework is in charge of controlling the access to network capabilities by Applications, as well the integrity of the operator’s network.
* The rest are a set of SCFs, which abstract the network functionality so that Applications can use the network without needing to understand its underlying technology. These SCFs are mappable to network functionality, but the mapping is not standardized (note that the Framework functionality does not have a mapping).

Besides the OSA API between the OSA Gateway and the 3rd Party Applications, there is an OSA Internal API between the Framework and the rest of the OSA SCSs. This Internal API supports multi-vendorship for the OSA SCSs, as well as the possibility that the Framework functionality is a business in itself.

Et nøkkelkrav sett fra nettoperatør og tjenesteleverandørs side, er å sikre at de definerte APIer ikke utsetter underliggende kommunikasjonsnett for uautorisert bruk eller trusler. Benytter derfor et rammeverk, OSA. Rammeverket består av en programvarekomponent som ved hjelp av krypteringsmetodikk autentiserer applikasjoner og returnerer objektreferanser til tillatte tjenestemuligheter.

![Parlay/OSA i nettet](https://github.com/kjbekkelund/ttm4130/raw/master/images/parlay-osa-overview.png)

![Registrering, avdekking og bruk av tjeneste](https://github.com/kjbekkelund/ttm4130/raw/master/images/parlay-sequence.png)

Parlay/OSA innfører et nytt nettelement, en gateway, som blir benyttet for å koble applikasjoner som benytter OSA APIer til den eksisterende infrastruktur. Denne overgangsenheten kontrolleres av nettoperatør eller tjenesteleverandør og representerer et enkelt punkt som alt samvirke med Parlay/OSA funksjonalitet må passere gjennom. Dette betyr at applikasjonene er atskilt fra de spesielle protokollene som benyttes i selve nettet. Derfor kan de anvendes uten innvirkning på allerede eksiterende applikasjoner og tjenester.

Fordi Parlay/OSA baserte tjenester kan utvikles ved bruk av standard programvareverktøy, så kan utviklere med bakgrunn i C++, CORBA, Java og EJB lett realisere dem.

![Open service architecture with API interfaces](https://github.com/kjbekkelund/ttm4130/raw/master/images/osa.png)

Be able to describe the philosophy behind/principles of “stateless network control” and why it is desirable to implement a service logic mainly based on this principle
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Vil man egentlig ha _kun_ et stupid network? Begge deler har sine positive og negative sider. Reliability, Accounting og QoS er fortsatt bedre i intelligente nett, mens det er mye raskere å innføre nye tjenester i dumme nettverk, samt at det dumme nettverks "likhet for alle" skaper større mulighet for innovasjon og nyskapning, noe som henger tett sammen med nettnøytralitet.

Know what a Media Gateway and a Media Gateway Controller is/does
----------------------------------------------------------------

A gateway is a device that is used to connect one type of network to another. The gateway's responsibility is to provide signalling interworking, as well as transforming the bearer information into a format that is suitable for the other network. A gateway may exist as a single monolithic device, or it may be decomposed into logical components:

* Signalling Gateway
* Media Gateway. Necessary in order to convert formats and QoS/resolution to terminals/networks, and to split or merge media streams when it is necessary.
* Media Gateway Controller. The part that controls the call control. 

Megaco (H.248) is an implementation of the Media Gateway Control Protocol architecture for controlling Media Gateways on IP networks and PSTN. Defines the protocol for Media Gateway Controllers to control Media Gateways for the support of multimedia streams across computer networks. Typically used to provide VoIP services between IP networks and PSTN, or entirely within IP networks.

H.248 contains a framwork that enables one to define control measures for media streams. It is a distributed media gateway. 

H.248 defines the protocols used between elements of a physically decomposed multimedia gateway.

**Termination**. Local entity on a MG that Sources and/or sinks one or more streams.

**Context**. Association between a collection of Terminations. Describes the topology and the media mixing and/or switching parameters is more than two Terminations are involved in an association. The _null_ context contains all Terminations that are not associated with any other Termination. A termination shall only exist in one Context ar a time.

![Example of Megaco Connection Model](https://github.com/kjbekkelund/ttm4130/raw/master/images/mgw.png)

**Sample topologies**

1. No topology descriptors. All Terminations have a bothway connection to all other Terminations.</td>
2. T1, T2 Isolate. Removes the connection between T1 and T2. T3 has a bothway connection with both T1 and T2. T1 and T2 have bothway connection to T3.</td>
3. T3, T2 oneway. A oneway connection from T3 to T2 (i.e. T2 receives media flow from T3). A bothway connection between T1 and T3.</td>
4. T2, T3 oneway. A oneway connection between T2 to T3. T1 and T3 remain bothway connected.</td>
5. T2, T3 bothway. T2 is bothway connected to T3. This results in the same as 2.</td>
6. T1, T2 bothway (T2, T3 bothway and T1, T3 bothway may be implied or explicit). All Terminations have a bothway connection to all other Terminations.</td>

![Sample Topologies](https://github.com/kjbekkelund/ttm4130/raw/master/images/sample-topologies.png)

Noen ikke-plasserte notater
---------------------------

* Et fremtidig heterogent nett vil få et omfattende behov for overganger (gateways) for overføring av både styresignaler og "nyttesignaler" mellom forskjelllige deler av nettet.
* Mål for framtidig tjenesteutvikling er å etablere et fleksiblet verktøysett av "tjenestemoduler" som kan instansieres/plugges inn i nettverket etter behov. Dette setter store krav til spesifisering og standardisering.