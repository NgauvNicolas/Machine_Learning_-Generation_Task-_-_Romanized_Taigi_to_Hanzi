# Machine_Learning_-Generation_Task-_-_Romanized_Taigi_to_Hanzi
## Système de conversion et de génération pour passer du taïwanais (taigi) romanisé à un texte en sinogrammes


### Essayer d'imaginer un système de conversion pour passer du taïwanais (taigi) romanisé  à un texte en sinogrammes.

### On peut imaginer deux scenarios :
- convertir toute la chaîne de caractères directement ou
- fonctionner comme une méthode de saisie pour prédire le caractère ou le mot suivant étant donnés une séquence romanisée et un contexte gauche correspondant au texte déjà converti.

### Objectif général du projet :
- Développer un système capable de convertir automatiquement une séquence romanisée (taigi) en sinogrammes chinois, à l’aide de techniques supervisées de machine learning / deep learning.






### Proposition de Plan de projet détaillé :
#### Phase 1 – Préparation des données
    - 1.1 Analyse du corpus JSONL
        - Charger le fichier training_sample.jsonl.
        - Vérifier que chaque ligne contient les champs "r" (romanisé) et "j" (sinogrammes).
    - 1.2 Nettoyage et normalisation
        - Supprimer ou marquer les cas ambigus ou mal alignés si nécessaire.
        - Standardiser la casse, ponctuation, caractères spéciaux.
    - 1.3 Tokenisation
        - Tokeniser les chaînes "r" en syllabes (ou en caractères).
        - Tokeniser les chaînes "j" en sinogrammes (caractères chinois individuels).
    - 1.4 Construction des vocabulaires
        - Créer deux dictionnaires :
            - token_to_index_r pour les tokens romanisés.
            - token_to_index_j pour les sinogrammes.
        - Gérer des tokens spéciaux : PAD, BOS, EOS, UNK.
#### Phase 2 – Vectorisation des données
    - 2.1 Transformation en séquences d’indices
        - Chaque entrée "r" devient une séquence d’indices pour l’encodeur.
        - Chaque sortie "j" devient une séquence cible pour le décodeur.
    - 2.2 Padding et alignement
        - Appliquer un pad_sequences() pour uniformiser la longueur des séquences (entrée et sortie).
        - Ajouter des marqueurs BOS et EOS si nécessaire.
#### Phase 3 – Construction du modèle
    - 3.1 Choix de l’architecture : seq2seq avec LSTM
        - Encoder :
            - Embedding + LSTM (retourne state_h, state_c)
        - Decoder :
            - Embedding + LSTM + Dense avec softmax
            - Utilisation de teacher forcing pour l’entraînement
    - 3.2 Compilation
        - Optimiseur : Adam
        - Fonction de perte : sparse_categorical_crossentropy
        - Métriques : accuracy (char-level)
#### Phase 4 – Entraînement du modèle
    - 4.1 Split des données
        - Séparer en train, validation et test (ex. 80/10/10)
    - 4.2 Lancement de l’entraînement
        - Utiliser model.fit() sur les données vectorisées.
        - Ajouter un EarlyStopping si besoin.
    - 4.3 Courbes d’apprentissage
        - Afficher loss et accuracy pour train/val avec matplotlib.
#### Phase 5 – Génération et inférence
    - 5.1 Implémentation d’un decode_sequence(input_seq)
        - Récupérer l’état de l’encodeur.
        - Générer un caractère à la fois, jusqu’au EOS.
    - 5.2 Conversion sur de nouvelles entrées
        - Tester la conversion sur des phrases romanisées inconnues.
#### Phase 6 – Évaluation
    - 6.1 Métriques automatiques
        - Calcul du taux d’erreur caractère à caractère.
        - Calcul du score BLEU entre sorties générées et vraies chaînes.
    - 6.2 Baseline de comparaison
        - Implémenter une version simple lookup mot-à-mot pour comparer.
#### Phase 7 – Documentation et analyse
    - 7.1 Analyse des erreurs
        - Étudier les erreurs fréquentes (ex. ambiguïté de syllabes).
        - Étudier la longueur des séquences vs. précision.
    - 7.2 Documentation du projet
        - Présenter les étapes, choix de modèles, résultats et perspectives.
        - Ajouter schémas, architecture, visualisations.
#### Phase 8 – Scénario B : Méthode de saisie / prédiction (Second Modèle, plus complexe)
    - Implémentation autoregressive
        - Transformer le problème en prédiction d’un caractère donné un contexte sinographique + romanisé.
        - Utiliser un modèle de type GPT-like simplifié ou LSTM autoregressif.
