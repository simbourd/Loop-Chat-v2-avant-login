# 🚀 Module d'Onboarding d'Agent IA - Loop Chat

## 🎯 Objectif
Créer un agent IA en 2 minutes avec une configuration ultra-simple :
1. L'admin colle son URL webhook (Make.com/n8n)
2. Le système génère l'URL de réponse sécurisée
3. L'admin colle cette URL dans son scénario
4. ✅ L'agent est opérationnel !

## 🏗️ Architecture

### Base de données
```sql
-- Table agents_loopchat_2024 (mise à jour)
ALTER TABLE agents_loopchat_2024 ADD COLUMN:
- webhook_url_in TEXT    -- URL du webhook Make/n8n
- webhook_url_out TEXT   -- URL générée par Loop Chat
- api_key TEXT          -- Clé API sécurisée
- is_active BOOLEAN     -- État de l'agent

-- Table api_keys_loopchat_2024 (nouvelle)
- id UUID PRIMARY KEY
- agent_id TEXT
- api_key TEXT
- created_at TIMESTAMP
- is_active BOOLEAN

-- Table webhook_messages_loopchat_2024 (nouvelle)
- id UUID PRIMARY KEY
- agent_id TEXT
- message_content TEXT
- sender_data JSONB
- processed BOOLEAN
- response_sent BOOLEAN
- created_at TIMESTAMP
```

### Endpoints sécurisés
```
POST /functions/v1/webhook-response/{agent_id}
Headers: Authorization: Bearer {api_key}
```

## 🔧 Composants

### 1. AgentOnboarding.jsx
Interface d'onboarding en 2 étapes :
- **Étape 1** : Configuration (nom, description, URL webhook, apparence)
- **Étape 2** : Résultats (URL de réponse, clé API, instructions)

### 2. Fonction Edge Supabase
- Réception des réponses webhook
- Vérification de la clé API
- Stockage des messages
- Mise à jour des statistiques

### 3. Services mis à jour
- `agentsService` : Support des nouveaux champs
- `apiKeysService` : Gestion des clés API
- `webhookMessagesService` : Traitement des messages webhook

## 🚀 Flux d'utilisation

### Côté administrateur
1. Clic sur le bouton "⚡" dans la sidebar
2. Remplir le formulaire :
   - Nom de l'agent
   - Description (optionnel)
   - URL webhook Make/n8n
   - Couleur et icône
3. Clic "Créer l'Agent"
4. Copier l'URL de réponse générée
5. Coller dans le scénario Make/n8n

### Côté technique
1. Génération automatique :
   - ID unique de l'agent
   - Clé API sécurisée (`lc_` + hash)
   - URL de réponse (`/webhook-response/{agent_id}`)
2. Stockage en base de données
3. Activation immédiate

## 🔒 Sécurité

### Authentification
- Chaque agent a sa propre clé API
- Header `Authorization: Bearer {api_key}` obligatoire
- Rotation des clés possible

### Validation
- Vérification de l'agent et de la clé API
- Messages stockés avec horodatage
- Logs d'activité complets

### HTTPS
- Toutes les communications sécurisées
- Endpoints Supabase Edge Functions
- Pas d'exposition de données sensibles

## 📋 Instructions Make.com/n8n

### Configuration du webhook entrant
1. Créer un webhook trigger
2. Coller l'URL fournie par l'admin
3. Configurer la réponse JSON

### Configuration du webhook sortant
1. Ajouter un module HTTP Request
2. URL : `{URL_REPONSE_GENEREE}`
3. Méthode : POST
4. Headers : `Authorization: Bearer {CLE_API}`
5. Body : JSON avec `response` et `conversation_id`

## 🧪 Test et validation

### Test automatique
- Bouton "Test" dans l'onboarding
- Vérification de la connectivité webhook
- Validation en temps réel

### Monitoring
- Statistiques d'utilisation
- Logs des messages
- État de connexion en temps réel

## 🎨 Interface utilisateur

### Design
- Interface moderne et intuitive
- Animations fluides (Framer Motion)
- Feedback visuel immédiat
- Responsive design

### Expérience utilisateur
- Processus guidé étape par étape
- Copie en un clic
- Instructions claires
- Aperçu en temps réel

## 🔄 Maintenance

### Rotation des clés API
- Interface dans les paramètres
- Désactivation de l'ancienne clé
- Génération automatique de la nouvelle

### Monitoring
- Dashboard des agents actifs
- Statistiques d'utilisation
- Alertes de dysfonctionnement

---

✅ **Résultat** : Création d'un agent IA fonctionnel en moins de 2 minutes avec une sécurité robuste et une expérience utilisateur optimale !