# Teknisk Spesifikasjon - "Hva er Kongsberg"

## Systemarkitektur

### Frontend
- **Rammeverk:** React.js
- **Styling:** Tailwind CSS
- **State Management:** Redux eller Context API
- **Routing:** React Router
- **UI Components:** Custom komponenter med fokus på brukervennlighet

### Backend
- **Plattform:** Node.js
- **API Framework:** Express.js
- **Autentisering:** JWT tokens, Vipps Login API
- **Bildebehandling:** Multer for opplasting, Sharp for optimalisering
- **Sikkerhet:** Helmet.js, rate limiting, CORS

### Database
- **Type:** MongoDB (NoSQL)
- **ODM:** Mongoose
- **Indeksering:** Optimalisert for søk og filtrering
- **Backup:** Daglige automatiserte backups

### Hosting & Deployment
- **Miljø:** AWS (Elastic Beanstalk eller EC2)
- **CI/CD:** GitHub Actions
- **Containerization:** Docker
- **SSL:** Let's Encrypt

## Datamodell

### Bruker
```json
{
  "_id": "ObjectId",
  "phoneNumber": "String",
  "email": "String",
  "firstName": "String",
  "lastName": "String",
  "createdAt": "Date",
  "lastLogin": "Date",
  "isActive": "Boolean",
  "isAdmin": "Boolean"
}
```

### Innlegg
```json
{
  "_id": "ObjectId",
  "userId": "ObjectId",
  "title": "String",
  "content": "String",
  "imageUrl": "String?",
  "category": "String",
  "createdAt": "Date",
  "updatedAt": "Date",
  "isApproved": "Boolean",
  "approvedAt": "Date?",
  "votes": {
    "up": "Number",
    "down": "Number"
  }
}
```

### Stemme
```json
{
  "_id": "ObjectId",
  "postId": "ObjectId",
  "userId": "ObjectId",
  "voteType": "Enum(UP, DOWN)",
  "createdAt": "Date"
}
```

## API Endepunkter

### Autentisering
- `POST /api/auth/vipps/initiate` - Start Vipps-pålogging
- `GET /api/auth/vipps/callback` - Callback fra Vipps
- `POST /api/auth/refresh` - Forny JWT token
- `POST /api/auth/logout` - Logg ut

### Brukere
- `GET /api/users/me` - Hent pålogget brukers info
- `PATCH /api/users/me` - Oppdater pålogget brukers info

### Innlegg
- `GET /api/posts` - List alle godkjente innlegg (med paginering)
- `GET /api/posts/:id` - Hent ett spesifikt innlegg
- `POST /api/posts` - Opprett nytt innlegg
- `PATCH /api/posts/:id` - Oppdater innlegg (kun eier)
- `DELETE /api/posts/:id` - Slett innlegg (kun eier eller admin)

### Stemmer
- `POST /api/posts/:id/vote` - Stem på innlegg
- `DELETE /api/posts/:id/vote` - Fjern stemme

### Admin
- `GET /api/admin/posts` - List alle innlegg (inkludert ikke-godkjente)
- `PATCH /api/admin/posts/:id/approve` - Godkjenn innlegg
- `PATCH /api/admin/posts/:id/reject` - Avvis innlegg
- `GET /api/admin/users` - List alle brukere
- `PATCH /api/admin/users/:id` - Oppdater bruker

## AI-filtrering

### Funksjonalitet
- Automatisk vurdering av innlegg før publisering
- Filtrering basert på upassende innhold, spam og sjikane
- Flerspråklig støtte (norsk primært, men også engelsk)
- Kontinuerlig læring basert på moderatorenes godkjenninger/avvisninger

### Implementasjon
- TensorFlow.js eller liknende ML-bibliotek
- Pre-trent modell for norsk tekstanalyse
- Moderatorgrensesnitt for å korrigere feilklassifiseringer
- Vektet scoring-system for ulike typer innhold

## Vipps-integrasjon

### Funksjonalitet
- Bruk av Vipps Login API for autentisering
- Sikker håndtering av brukerinfo
- Kontaktverifisering via telefonnummer

### Sikkerhet
- Lagring av brukerdata i henhold til GDPR
- Kryptering av personlig identifiserbar informasjon
- Regelmessig revisjon av autentiseringssystemet

## QR-kode System

### Generering
- Unike QR-koder for ulike lokasjoner i Kongsberg
- Inkludering av UTM-parametere for sporing
- Visuelt tilpasset design med "Hva er Kongsberg" branding

### Analyse
- Sporing av skanninger per lokasjon
- Konverteringsrater fra skanning til pålogging
- A/B-testing av plassering og design

## Skalerbarhet og Ytelse

### Optimalisering
- Lazy loading av bilder
- CDN for statiske ressurser
- Caching-strategi for hyppig besøkte endepunkter
- Database-indeksering for vanlige spørringer

### Skalering
- Auto-scaling basert på trafikk
- Lastbalansering
- Minneoptimalisering

## Vedlikehold og Overvåking

### Monitorering
- Logging med ELK Stack (Elasticsearch, Logstash, Kibana)
- Ytelsesovervåking med Prometheus
- Feilrapportering med Sentry

### Sikkerhetsoppdateringer
- Automatisert sårbarhetsscanning
- Regelmessig dependency-sjekk
- Sikkerhetspatcher innen 48 timer

## Migreringsstrategi

### Database
- Versjonskontroll av skjemaer
- Inkrementelle migreringer
- Rollback-planer

### Applikasjon
- Blue-Green deployment
- Feature toggles
- Gradvis utrulling
