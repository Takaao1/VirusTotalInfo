#!/bin/bash

# Configuration : Liste des clés API
API_KEYS=("key-1" "key-2" "key-3")
REQUESTS_PER_KEY=5    # Nombre de requêtes par clé avant rotation
DELAY_BETWEEN_REQUESTS=20  # Délai entre chaque requête (en secondes)

# Variables globales
current_key_index=0
request_count=0

# Fonction pour afficher l'aide
display_help() {
  echo -e "\033[1;32mUsage:\033[0m"
  echo -e "  $0 <domain>           # Vérifier un domaine unique"
  echo -e "  $0 -f <file>          # Vérifier plusieurs domaines depuis un fichier"
  echo -e "  $0 -h                 # Afficher cette aide"
  exit 0
}

# Fonction pour gérer les erreurs
handle_error() {
  local message=$1
  echo -e "\033[1;31mErreur:\033[0m $message"
  exit 1
}

# Fonction pour obtenir la clé API actuelle
get_current_api_key() {
  echo "${API_KEYS[$current_key_index]}"
}

# Fonction pour changer de clé API
rotate_api_key() {
  current_key_index=$(( (current_key_index + 1) % ${#API_KEYS[@]} ))
  request_count=0
}

# Fonction pour appeler l'API et afficher les URLs non détectées
fetch_undetected_urls() {
  local domain=$1
  local api_key=$(get_current_api_key)
  local url="https://www.virustotal.com/vtapi/v2/domain/report?apikey=$api_key&domain=$domain"

  echo -e "\nFetching data for domain: \033[1;34m$domain\033[0m (using API key: $((current_key_index + 1)))"
  
  response=$(curl -s "$url")
  if [[ $? -ne 0 || -z "$response" ]]; then
    echo -e "\033[1;31mErreur lors de la récupération des données pour le domaine: $domain\033[0m"
    return
  fi

  # Extraction des URLs non détectées
  undetected_urls=$(echo "$response" | jq -r '.undetected_urls[][0]' 2>/dev/null)
  if [[ -z "$undetected_urls" ]]; then
    echo -e "\033[1;33mAucune URL non détectée trouvée pour le domaine: $domain\033[0m"
  else
    echo -e "\033[1;32mURLs non détectées pour le domaine: $domain\033[0m"
    echo "$undetected_urls"
  fi

  # Gestion des compteurs
  request_count=$((request_count + 1))
  if [[ $request_count -ge $REQUESTS_PER_KEY ]]; then
    echo -e "\033[1;36mRotation de la clé API...\033[0m"
    rotate_api_key
  fi
}

# Fonction pour afficher un compte à rebours
countdown() {
  local seconds=$1
  while [ $seconds -gt 0 ]; do
    echo -ne "\033[1;36mAttente de $seconds secondes...\033[0m\r"
    sleep 1
    seconds=$((seconds - 1))
  done
  echo -ne "\033[0K"  # Effacer la ligne
}

# Vérification des arguments
if [[ $# -eq 0 ]]; then
  display_help
fi

# Mode fichier ou domaine unique
if [[ $1 == "-f" && -n $2 ]]; then
  file=$2
  if [[ ! -f $file ]]; then
    handle_error "Le fichier spécifié n'existe pas : $file"
  fi

  # Lire les domaines dans le fichier
  while IFS= read -r domain; do
    domain=$(echo "$domain" | sed 's|https\?://||')
    fetch_undetected_urls "$domain"
    countdown $DELAY_BETWEEN_REQUESTS
  done < "$file"
elif [[ $1 == "-h" ]]; then
  display_help
else
  domain=$(echo "$1" | sed 's|https\?://||')
  fetch_undetected_urls "$domain"
fi

echo -e "\033[1;32mTerminé !\033[0m"
