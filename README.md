-swagger: "2.0"
info:
  description: "API for trumf-partnere"
  version: "2.5"
  title: "trumf-partnere"
host: "preprod.service-dk.norgesgruppen.no"
basePath: "/partnere"
tags:
- name: "Eksterne kredittkort"
- name: "Partner identifikator."
- name: "Partner Samtykker"
- name: "Tjeneste for Butikker fra partnere."
- name: "Tjeneste for henting av medlemInfo for partnere"
- name: "Tjeneste for transaksjoner fra partnere mot Trumf."
- name: "Tjeneste for varekategorier fra partnere."
- name: "Trumf Medlemid"
- name: "Trumfvisa søknad"
schemes:
- "https"
paths:
  /butikker:
    post:
      tags:
      - "Tjeneste for Butikker fra partnere."
      summary: "Opprett eller oppdater butikker"
      description: ""
      operationId: "opprettButikker"
      parameters:
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/ButikkOppdatering"
      responses:
        200:
          description: "Butikker er oppdatert"
        400:
          description: "Manglende eller ugyldig inputdata"
          schema:
            $ref: "#/definitions/JsonError"
        401:
          description: "Mangler token, utgått token eller ugyldig token"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam:
        - "NGT-B2B.MesterGronn"
        - "NGT-B2B.intern"
  /eksternekredittkort/opprett/{medlemid}:
    post:
      tags:
      - "Eksterne kredittkort"
      summary: "Opprett Eksternt kredittkort"
      description: "Oppretter nytt kredittkort for medlem"
      operationId: "opprettEksterntKredittkort"
      parameters:
      - name: "medlemid"
        in: "path"
        required: true
        type: "string"
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/EksterntKredittkort"
      responses:
        200:
          description: "OK"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam: []
  /eksternekredittkort/slett/{medlemid}:
    post:
      tags:
      - "Eksterne kredittkort"
      summary: "Slett Eksternt kredittkort"
      description: "Sletter kredittkort for medlem"
      operationId: "slettEksterntKredittkort"
      parameters:
      - name: "medlemid"
        in: "path"
        required: true
        type: "string"
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/EksterntKredittkort"
      responses:
        200:
          description: "OK"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam: []
  /health:
    get:
      operationId: "health"
      parameters: []
      responses:
        default:
          description: "successful operation"
  /identifikatorer:
    post:
      tags:
      - "Partner identifikator."
      summary: "Oppretter partnerIdentifikator på medlem."
      description: "Lagrer partners interne kundeid partner har tilknyttet ett Trumf-medlem"
      operationId: "registrerPartnerIdentifikator"
      parameters:
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/PartnerIdentifikator"
      responses:
        200:
          description: "OK"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam: []
    put:
      tags:
      - "Partner identifikator."
      summary: "Oppdaterer partnerIdentifikator på medlem."
      description: "Oppdaterer partners interne kundeid på Trumf-medlem"
      operationId: "oppdaterPartnerIdentifikator"
      parameters:
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/PartnerIdentifikator"
      responses:
        200:
          description: "OK"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam: []
  /identifikatorer/{type}/verdier/{partneridentifikator}:
    delete:
      tags:
      - "Partner identifikator."
      summary: "Sletter partnerIdentifikator på medlem."
      description: "Sletter partners interne kundeid fra Trumf-medlem"
      operationId: "slettPartnerIdentifikator"
      parameters:
      - name: "type"
        in: "path"
        required: true
        type: "string"
      - name: "partneridentifikator"
        in: "path"
        required: true
        type: "string"
      - name: "partnerid"
        in: "query"
        required: true
        type: "string"
      responses:
        200:
          description: "OK"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam: []
  /medleminfo/{medlemid}:
    get:
      tags:
      - "Tjeneste for henting av medlemInfo for partnere"
      summary: "Henter medlemsinfo"
      description: "Henter info om medlem for partner. Informasjon som returneres\
        \ filtreres basert på partneridDersom partner ikke har leserettigheter for\
        \ felter så vil disse ikke returneres fra endepunktet."
      operationId: "hentMedlemInfo"
      parameters:
      - name: "medlemid"
        in: "path"
        required: true
        type: "string"
      - name: "partnerid"
        in: "query"
        required: true
        type: "string"
      responses:
        200:
          description: "OK"
          schema:
            $ref: "#/definitions/MedlemInfo"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/JsonError"
  /samtykker:
    get:
      tags:
      - "Partner Samtykker"
      summary: "Henter samtykker for partner."
      description: "Kun samtykker som partner har lese/skrivetilgang på returneres."
      operationId: "hentSamtykker"
      parameters:
      - name: "partnerid"
        in: "query"
        required: true
        type: "string"
      - name: "medlemid"
        in: "query"
        required: true
        type: "string"
      responses:
        200:
          description: "OK"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam: []
    put:
      tags:
      - "Partner Samtykker"
      summary: "Oppdaterer samtykker for partner."
      description: "Kun samtykker som partner har lese/skrivetilgang på kan oppdateres."
      operationId: "oppdaterSamtykke"
      parameters:
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/Samtykke"
      responses:
        200:
          description: "OK"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam: []
  /transaksjoner:
    post:
      tags:
      - "Tjeneste for transaksjoner fra partnere mot Trumf."
      summary: "Opprett en transaksjon"
      description: ""
      operationId: "opprettTransaksjon"
      parameters:
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/OpprettTransaksjonerDto"
      responses:
        200:
          description: "Informasjon om opprettet transaksjon"
          schema:
            $ref: "#/definitions/TransaksjonOpprettetDto"
        400:
          description: "Manglende eller ugyldig inputdata"
          schema:
            $ref: "#/definitions/JsonError"
        401:
          description: "Mangler token, utgått token eller ugyldig token"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam: []
  /transaksjoner/pending:
    post:
      tags:
      - "Tjeneste for transaksjoner fra partnere mot Trumf."
      summary: "Opprett en pending transaksjon"
      description: "Transaksjon som blir forhåndsmeldt, pending transaksjoner vil\
        \ ikke bli bonusberegnet. Skal bare benyttes for å melde inn transaksjoner\
        \ som avventer behandling av partnere før bonusberegning."
      operationId: "opprettPendingTransaksjon"
      parameters:
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/OpprettTransaksjonerDto"
      responses:
        200:
          description: "Informasjon om opprettet pending transaksjon"
          schema:
            $ref: "#/definitions/TransaksjonOpprettetDto"
        400:
          description: "Manglende eller ugyldig inputdata"
          schema:
            $ref: "#/definitions/JsonError"
        401:
          description: "Mangler token, utgått token eller ugyldig token"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam: []
  /transaksjoner/{transaksjonsId}:
    get:
      tags:
      - "Tjeneste for transaksjoner fra partnere mot Trumf."
      summary: "Henter transaksjonsDetalj"
      description: "Henter transaskjonsDetalj for en gitt transaksjonsId og partner.\
        \ En partner kan kun hente detaljer for transaksjoner de har rapportert."
      operationId: "hentPartnerTransaksjonsDetalj"
      parameters:
      - name: "transaksjonsId"
        in: "path"
        required: true
        type: "string"
      - name: "partner"
        in: "query"
        required: true
        type: "string"
      responses:
        200:
          description: "OK"
          schema:
            $ref: "#/definitions/PartnerTransaksjonsDetalj"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/JsonError"
        401:
          description: "Mangler token, utgått token eller ugyldig token"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam: []
  /trumfmedlemid:
    get:
      tags:
      - "Trumf Medlemid"
      summary: "Henter Trumf medlemId"
      description: "Slår opp Trumf medlemId for et medlem basert på id-type og id."
      operationId: "hentMedlemId"
      parameters:
      - name: "id"
        in: "query"
        required: true
        type: "string"
      - name: "type"
        in: "query"
        required: true
        type: "string"
      responses:
        200:
          description: "OK"
          schema:
            $ref: "#/definitions/TrumfMedlemId"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam: []
  /trumfvisasokere:
    post:
      tags:
      - "Trumfvisa søknad"
      summary: "Registrerer at medlem har søkt om Trumf visa"
      description: "Mottar registrering på at ett medlem har søkt om Trumf Visa kort"
      operationId: "registrerTrumfvisaSoker"
      parameters:
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/TrumfvisaSoker"
      responses:
        200:
          description: "OK"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/JsonError"
  /trumfvisasokere/medleminfo/{medlemId}:
    get:
      tags:
      - "Trumfvisa søknad"
      summary: "Henter medlem-info på Trumfvisa søkere."
      description: "Tjenesten leverer ut medlem-info for behandling av Trumfvisa sø\
        knad"
      operationId: "hentTrumfvisaSokerInfo"
      parameters:
      - name: "medlemId"
        in: "path"
        required: true
        type: "string"
      responses:
        200:
          description: "OK"
          schema:
            $ref: "#/definitions/TrumfVisaSokerInfo"
        400:
          description: "BAD REQUEST"
          schema:
            $ref: "#/definitions/SapFeil"
      security:
      - oam: []
  /varekategorier:
    post:
      tags:
      - "Tjeneste for varekategorier fra partnere."
      summary: "Opprett eller oppdater varekategorier"
      description: ""
      operationId: "opprettVarekategorier"
      parameters:
      - in: "body"
        name: "body"
        required: true
        schema:
          $ref: "#/definitions/VarekategoriOppdatering"
      responses:
        200:
          description: "Kategorier er oppdatert"
        400:
          description: "Manglende eller ugyldig inputdata"
          schema:
            $ref: "#/definitions/JsonError"
        401:
          description: "Mangler token, utgått token eller ugyldig token"
          schema:
            $ref: "#/definitions/JsonError"
      security:
      - oam:
        - "NGT-B2B.MesterGronn"
        - "NGT-B2B.intern"
securityDefinitions:
  oam:
    type: "oauth2"
    authorizationUrl: "https://oamtest.norgesgruppen.no/ms_oauth/oauth2/endpoints/oauthservice/tokens"
    flow: "implicit"
    scopes:
      NGT-B2B.info: "Info scope"
      NGT-B2B.eidsivaenergi: "Partnerspesifict scope"
      NGT-B2B.gudbrandsdalenergi: "Partnerspesifict scope"
      NGT-B2B.MesterGronn: "Partnerspesifict scope"
      NGT-B2B.intern: "Internt scope for NGT"
definitions:
  Butikk:
    type: "object"
    required:
    - "butikkId"
    - "butikkNavn"
    - "by"
    - "epost"
    - "gate"
    - "postnummer"
    - "telefonnummer"
    properties:
      butikkId:
        type: "string"
        description: "Id for butikk som skal opprettes eller endres"
      butikkNavn:
        type: "string"
        description: "Navnet på butikken"
      gate:
        type: "string"
        description: "Gateadresse for butikken"
      postnummer:
        type: "string"
        description: "Postnummer. 4 siffer"
      by:
        type: "string"
        description: "By"
      telefonnummer:
        type: "string"
        description: "Gyldig telefonnummer. 8 Siffer"
      epost:
        type: "string"
        description: "Gyldig epostadresse"
  ButikkOppdatering:
    type: "object"
    required:
    - "butikker"
    - "partnerId"
    properties:
      partnerId:
        type: "string"
        description: "PartnerId for Partner som skal oppdatere kategorier"
      butikker:
        type: "array"
        description: "Liste med butikker som skal opprettes eller oppdateres"
        items:
          $ref: "#/definitions/Butikk"
  EksterntKredittkort:
    type: "object"
    properties:
      maskertKortnummer:
        type: "string"
        description: "Maskert kornummer for kortet som skal registreres eller slettes"
      utlopsDato:
        type: "string"
        format: "date"
        description: "Utløpsdato for kortet som skal registreres eller slettes"
      partner:
        type: "string"
        description: "Partnerid for partner som utsteder kredittkort"
  JsonError:
    type: "object"
    properties:
      code:
        type: "integer"
        format: "int32"
      reason:
        type: "string"
      message:
        type: "string"
      messageCode:
        type: "integer"
        format: "int32"
  MedlemId:
    type: "object"
  MedlemInfo:
    type: "object"
    properties:
      medlemId:
        type: "string"
        description: "Medlemmets Trumf-id"
        readOnly: true
      aeraConsumerId:
        type: "string"
        description: "Medlemmets Consumerid i Aera"
        readOnly: true
      fornavn:
        type: "string"
        description: "Fornavn"
        readOnly: true
      etternavn:
        type: "string"
        description: "Etternavn"
        readOnly: true
      adresse:
        type: "string"
        description: "Adresse"
        readOnly: true
      postnr:
        type: "string"
        description: "Postnummer"
        readOnly: true
      poststed:
        type: "string"
        description: "Poststed"
        readOnly: true
      mobil:
        type: "string"
        description: "Mobiltelefonnummer"
        readOnly: true
      epost:
        type: "string"
        description: "Epost-adresse"
        readOnly: true
      saldo:
        type: "string"
        description: "Medlemmet Trumf-saldo"
        readOnly: true
      kundenummer:
        type: "string"
        description: "Medlemmets kundenummer."
        readOnly: true
  OpprettTransaksjonerDto:
    type: "object"
    required:
    - "partner"
    - "partnersTransaksjonsId"
    - "transaksjonsdato"
    - "transaksjonstid"
    properties:
      partner:
        type: "string"
        description: "Partneren som oppretter transaksjon."
      medlemsnummer:
        type: "string"
        description: "Id til Trumf-medlemmet. Må være numerisk dersom satt."
      kanal:
        type: "string"
        description: "Hvordan transaksjonen ble gjennomført. Eks. POS, app."
      mediatype:
        type: "string"
        description: "Hvordan brukeren ble identifisert. Eks: bankkonto, TrumfVisa,\
          \ Shell Mastercard."
      medianummer:
        type: "string"
        description: "Knyttet til mediatype. Vedien av bankkontonummer, maskert kortnummer,\
          \ etc."
      transaksjonsdato:
        type: "string"
        description: "Dato for transaksjonen på format yyyyMMdd."
      transaksjonstid:
        type: "string"
        description: "Tidspunkt for transaksjonen på format HHmmss."
      partnersTransaksjonsId:
        type: "string"
        description: "Intern transksjonsId for partner."
      brukerstedsId:
        type: "string"
      brukerstedsnavn:
        type: "string"
      kilde:
        type: "string"
      mcckode:
        type: "string"
      ordreId:
        type: "string"
      status:
        type: "string"
      transaksjonsdetaljer:
        type: "array"
        items:
          $ref: "#/definitions/TransaksjonsdetaljerDto"
  PartnerId:
    type: "object"
  PartnerIdentifikator:
    type: "object"
    required:
    - "identifikatorType"
    - "partner"
    - "partnerIdentifikator"
    properties:
      medlemId:
        type: "string"
        description: "medlemId for medlemmet som identifikator tilhører"
      partner:
        type: "string"
        description: "Partner som identifikator tilhører"
      identifikatorType:
        type: "string"
        description: "Navn på identifikator"
      partnerIdentifikator:
        type: "string"
        description: "Verdi på identifikator"
      gyldigfra:
        type: "string"
        format: "date"
        description: "Optional felt. Tidspunkt for når identifikatoren blir gyldig.\
          \ Format yyyy-MM-dd"
      gyldigtil:
        type: "string"
        format: "date"
        description: "Optional felt. Tidspunkt for når identifikatoren blir ugyldig.\
          \ Format yyyy-MM-dd"
  PartnerTransaksjonsDetalj:
    type: "object"
    properties:
      transaksjonsId:
        type: "string"
        description: "Partners transaksjonsId for transaksjon som skal hentes"
        readOnly: true
      partnerId:
        type: "string"
        description: "PartnerId"
        readOnly: true
      totalBonus:
        type: "string"
        description: "Total bonus på transaksjonen"
        readOnly: true
      transaksjonsLinjer:
        type: "array"
        description: "Liste med varelinjer for transaksjonen"
        readOnly: true
        items:
          $ref: "#/definitions/PartnerVarelinje"
  PartnerVarelinje:
    type: "object"
    properties:
      nummerSerieStatus:
        type: "string"
        description: "Internt løpenummer i NG"
        readOnly: true
      produktId:
        type: "string"
        description: "Id på produktet"
        readOnly: true
      belop:
        type: "string"
        description: "Beløp"
        readOnly: true
      trumfPoeng:
        type: "string"
        description: "TrumfPoeng opptjent på produktet"
        readOnly: true
      status:
        type: "string"
        description: "Status på linjen. 1=Mottat 2=behandlet"
        readOnly: true
  Samtykke:
    type: "object"
    properties:
      samtykkeId:
        type: "string"
        description: "Unik id for medlem/samtykke."
      partner:
        description: "PartnerId som samtykket er knyttet til."
        $ref: "#/definitions/PartnerId"
      medlemId:
        description: "MedlemId for medlemmet som har samtykket."
        $ref: "#/definitions/MedlemId"
      medlemGuid:
        type: "string"
        description: "Unik GUID for medlemmet."
      kanalKode:
        type: "string"
        description: "Kode som beskriver kommunikasjonskanal."
      kanal:
        type: "string"
        description: "Beskrivelse for kommunikasjonskanal."
      type:
        type: "string"
        description: "Samtykke type."
      status:
        type: "string"
        description: "Status på samtykket, Y/N/null."
      visningsTekst:
        type: "string"
        description: "Tekst som kan benyttes for visning av samtykket."
      sattDato:
        type: "string"
        format: "date"
        description: "Dato samtykket sist ble oppdatert."
      beskrivelse:
        type: "string"
        description: "Beskrivelse som kan brukes under visning av samtykket."
      avmeldingBeskrivelse:
        type: "string"
        description: "Beskrivelse som kan brukes dersom medlemmet melder seg av samtykket."
      distribusjonskanal:
        type: "string"
        description: "Distribusjonskanal for partner/kjede samtykket hører til."
  SapFeil:
    type: "object"
    properties:
      feilType:
        type: "string"
        readOnly: true
      sapMeldingsKode:
        type: "string"
        readOnly: true
      meldingVariabel1:
        type: "string"
        readOnly: true
      meldingVariabel2:
        type: "string"
        readOnly: true
      meldingVariabel3:
        type: "string"
        readOnly: true
      meldingVariabel4:
        type: "string"
        readOnly: true
      cid:
        type: "string"
        readOnly: true
  TransaksjonOpprettetDto:
    type: "object"
    properties:
      partnersTransaksjonsId:
        type: "string"
      internId:
        type: "string"
  TransaksjonsdetaljerDto:
    type: "object"
    properties:
      produktgruppe:
        type: "string"
      produktnummer:
        type: "string"
      produktbeskrivelse:
        type: "string"
      belop:
        type: "string"
        description: "Beløpet. Dersom desimaltall, skal punktum benyttes som separator\
          \ og det må være maks fire desimaler."
      enhet:
        type: "string"
        description: "Enhet. Maks tre tegn. Gyldige verdier ..."
      mengde:
        type: "string"
        description: "Mengde. Dersom desimaltall, skal punktum benyttes som separator\
          \ og det må være maks fem desimaler."
      trumfpoeng:
        type: "string"
        description: "Ferdig utregnet trumfpoeng. Dersom denne er utfylt, blir det\
          \ ikke foretatt beregning av poeng. Dersom desimaltall, skal punktum benyttes\
          \ som separator og det må være maks fem desimaler."
      ekstratrumf:
        type: "string"
        description: "Ekstra trumfpoeng. Dersom desimaltall, skal punktum benyttes\
          \ som separator og det må være maks fem desimaler."
      mvaprosent:
        type: "string"
        description: "MVA-sats i prosent.  Dersom desimaltall, skal punktum benyttes\
          \ som separator og det må være maks to desimaler."
      tilleggsbeskrivelse:
        type: "string"
        description: "Ekstra beskrivelse av transaksjonsdetalj"
      status:
        type: "string"
        description: "Status for transaksjonsdetalj."
  TrumfMedlemId:
    type: "object"
    properties:
      medlemId:
        type: "string"
        description: "Medlemmets Trumf-id"
  TrumfVisaSokerInfo:
    type: "object"
    properties:
      medlemId:
        type: "string"
        description: "Medlemmets Trumf-id"
        readOnly: true
      innmeldingsdato:
        type: "string"
        description: "Dato medlemmet ble meldt inn i Trumf"
        readOnly: true
      harTrumfvisa:
        type: "boolean"
        description: "True dersom medlemmet har registrert ett aktivt Trumf-visa kort"
        readOnly: true
      totaltAkkumulertSaldo:
        type: "string"
        description: "Totalt akkumulert Trumf-saldo for medlemmet siden innmelding"
        readOnly: true
  TrumfvisaSoker:
    type: "object"
    properties:
      medlemId:
        type: "string"
        description: "MedlemId for medlemmet som har søkt"
  Varekategori:
    type: "object"
    required:
    - "beskrivelse"
    - "hovedkategori"
    - "underkategori"
    properties:
      hovedkategori:
        type: "string"
        description: "Id for hovekategori"
      underkategori:
        type: "string"
        description: "Id for underkategori som skal opprettes eller oppdateres"
      beskrivelse:
        type: "string"
        description: "Beskrivelse for kategorien"
  VarekategoriOppdatering:
    type: "object"
    required:
    - "partnerId"
    - "varekategorier"
    properties:
      partnerId:
        type: "string"
        description: "PartnerId for Partner som skal oppdatere kategorier"
      varekategorier:
        type: "array"
        description: "Liste med kategorier som skal opprettes eller oppdateres"
        items:
          $ref: "#/definitions/Varekategori"
