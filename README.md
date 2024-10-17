# help-brevet
import tkinter as tk
from tkinter import messagebox
import random
import unicodedata

# Fonction pour normaliser les chaînes (retirer les accents, passer en minuscule)
def normaliser_chaine(chaine):
    # Convertit en forme Unicode NFD (décompose les caractères accentués en caractères de base et diacritiques séparés)
    chaine = unicodedata.normalize('NFD', chaine)
    # Supprime les diacritiques (accents)
    chaine = ''.join(c for c in chaine if unicodedata.category(c) != 'Mn')
    # Convertit en minuscule
    return chaine.lower()

# Questions de révision par matière
questions = {
    "mathématiques": [
        ("Quelle est la solution de l'équation 2x + 3 = 7 ?", "x = 2"),
        ("Quelle est la formule de l'aire d'un cercle ?", "A = πr²"),
        ("Quel est le PGCD de 36 et 24 ?", "12"),
        ("Combien de côtés a un hexagone ?", "6"),
        ("Quelle est la dérivée de x² ?", "2x"),
        ("Quelle est la valeur de π (Pi) approximée à deux décimales ?", "3.14"),
        ("Résous l'inéquation 3x - 5 < 10", "x < 5"),
        ("Quelle est la racine carrée de 81 ?", "9"),
        ("Que vaut 2⁵ ?", "32"),
        ("Quelle est la somme des angles d'un triangle ?", "180 degrés")
    ],
    "histoire": [
        ("En quelle année la Révolution française a-t-elle débuté ?", "1789"),
        ("Qui a été le premier président de la 5e République française ?", "Charles de Gaulle"),
        ("Quel est le nom du traité qui a mis fin à la Première Guerre mondiale ?", "Traité de Versailles"),
        ("Quand a eu lieu la bataille de Verdun ?", "1916"),
        ("Qui était Napoléon Bonaparte ?", "Un empereur français"),
        ("Quelle année marque la fin de la Seconde Guerre mondiale ?", "1945"),
        ("Qu'est-ce que la guerre froide ?", "Un conflit idéologique entre les États-Unis et l'URSS"),
        ("Quel roi a été guillotiné pendant la Révolution française ?", "Louis XVI"),
        ("Quel événement a déclenché la Première Guerre mondiale ?", "L'assassinat de l'archiduc François-Ferdinand"),
        ("Quelle est la date du débarquement en Normandie ?", "6 juin 1944")
    ],
    "géographie": [
        ("Quel est le plus long fleuve du monde ?", "Le Nil"),
        ("Quel est le pays le plus peuplé au monde ?", "La Chine"),
        ("Quel océan borde la côte est de l'Afrique ?", "Océan Indien"),
        ("Quel est le plus grand désert du monde ?", "Le Sahara"),
        ("Quelle est la capitale de l'Australie ?", "Canberra"),
        ("Quelle est la plus haute montagne du monde ?", "L'Everest"),
        ("Dans quel pays se trouve la forêt amazonienne ?", "Brésil"),
        ("Quel est le pays le plus vaste du monde ?", "La Russie"),
        ("Quels sont les cinq continents ?", "Afrique, Amérique, Asie, Europe, Océanie"),
        ("Quelle ville est la capitale du Japon ?", "Tokyo")
    ],
    "français": [
        ("Comment appelle-t-on un texte qui raconte une histoire ?", "Un récit"),
        ("Quelle est la différence entre un récit à la première personne et à la troisième personne ?", 
         "La première personne utilise 'je' et la troisième 'il' ou 'elle'"),
        ("Citez un auteur du 19e siècle.", "Victor Hugo"),
        ("Quel est le genre littéraire de 'Les Misérables' ?", "Roman"),
        ("Donnez un exemple de figure de style.", "Métaphore"),
        ("Qu'est-ce qu'une allitération ?", "La répétition de sons consonantiques"),
        ("Qui a écrit 'Le Cid' ?", "Corneille"),
        ("Qu'est-ce qu'une métaphore ?", "Une comparaison sans outil de comparaison"),
        ("Donnez un exemple de texte argumentatif.", "Un essai"),
        ("Comment appelle-t-on un poème en 14 vers ?", "Un sonnet")
    ],
    "sciences": [
        ("Quelle est la formule chimique de l'eau ?", "H2O"),
        ("Quel organe est responsable de la circulation du sang dans le corps ?", "Le cœur"),
        ("Quelle planète est la plus proche du Soleil ?", "Mercure"),
        ("Quelle est la distance approximative entre la Terre et le Soleil ?", "150 millions de km"),
        ("De quoi est composée la lumière blanche ?", "Un mélange de toutes les couleurs"),
        ("Quel est l'élément chimique représenté par le symbole 'O' ?", "Oxygène"),
        ("Quelle est la planète la plus grande du système solaire ?", "Jupiter"),
        ("Comment appelle-t-on l'unité de mesure de l'intensité électrique ?", "Ampère"),
        ("Quelle force nous maintient au sol ?", "La gravité"),
        ("Quelle est la durée de la révolution de la Terre autour du Soleil ?", "365 jours")
    ],
    "anglais": [
        ("How do you say 'chien' in English?", "dog"),
        ("What is the past tense of 'go'?", "went"),
        ("Translate 'Je suis étudiant' into English.", "I am a student"),
        ("What is the opposite of 'hot'?", "cold"),
        ("How do you say 'merci' in English?", "thank you"),
        ("What is the plural of 'child'?", "children"),
        ("Translate 'maison' into English.", "house"),
        ("What is the past tense of 'be'?", "was/were"),
        ("What is the English word for 'livre'?", "book"),
        ("What is the opposite of 'big'?", "small")
    ],
    "espagnol": [
        ("Comment dit-on 'bonjour' en espagnol ?", "hola"),
        ("Quel est le verbe 'être' au présent en espagnol ?", "ser"),
        ("Traduire 'Je m'appelle' en espagnol.", "Me llamo"),
        ("Comment dit-on 'merci' en espagnol ?", "gracias"),
        ("Quel est le verbe 'avoir' en espagnol ?", "tener"),
        ("Comment dit-on 'au revoir' en espagnol ?", "adiós"),
        ("Quelle est la capitale de l'Espagne ?", "Madrid"),
        ("Comment dit-on 's'il vous plaît' en espagnol ?", "por favor"),
        ("Comment conjugue-t-on 'hablar' à la première personne du singulier ?", "yo hablo"),
        ("Comment dit-on 'bonne nuit' en espagnol ?", "buenas noches")
    ]
}

def poser_question(matiere):
    question, reponse = random.choice(questions[matiere])
    return question, reponse

def verifier_reponse():
    reponse_utilisateur = normaliser_chaine(entree_reponse.get().strip())
    if reponse_utilisateur == normaliser_chaine(reponse_correcte.strip()):
        messagebox.showinfo("Résultat", "Correct ! Bien joué.")
    else:
        messagebox.showerror("Résultat", f"Incorrect. La bonne réponse était : {reponse_correcte}")
    entree_reponse.delete(0, tk.END)
    nouvelle_question()

def nouvelle_question():
    global reponse_correcte
    matiere = matiere_var.get()
    if matiere:
        question, reponse_correcte = poser_question(matiere)
        label_question.config(text=f"Question en {matiere.capitalize()} : {question}")

# Fonction pour ajouter des caractères spéciaux au champ de réponse
def ajouter_caractere(caractere):
    entree_reponse.insert(tk.END, caractere)

# Interface Graphique Tkinter
app = tk.Tk()
app.title("Quiz de Révision Brevet")

# Choix de la matière
matiere_var = tk.StringVar(value="mathématiques")
label_matiere = tk.Label(app, text="Choisis une matière :")
label_matiere.pack()

matiere_menu = tk.OptionMenu(app, matiere_var, *questions.keys())
matiere_menu.pack()

# Affichage de la question
label_question = tk.Label(app, text="Question :", wraplength=400)
label_question.pack(pady=20)

# Entrée pour la réponse de l'utilisateur
entree_reponse = tk.Entry(app, width=50)
entree_reponse.pack()

# Bouton pour vérifier la réponse
bouton_verifier = tk.Button(app, text="Vérifier la réponse", command=verifier_reponse)
bouton_verifier.pack(pady=10)

# Boutons pour les caractères spéciaux
frame_caracteres = tk.Frame(app)
frame_caracteres.pack(pady=10)

# Caractères mathématiques
label_math = tk.Label(frame_caracteres, text="Caractères mathématiques :")
label_math.grid(row=0, column=0, columnspan=3)

math_characters = ["π", "²", "³", "√", "+", "-", "×", "÷"]
for index, char in enumerate(math_characters):
    bouton_char = tk.Button(frame_caracteres, text=char, command=lambda c=char: ajouter_caractere(c))
    bouton_char.grid(row=1, column=index)

# Caractères espagnols
label_espanol = tk.Label(frame_caracteres, text="Caractères espagnols :")
label_espanol.grid(row=2, column=0, columnspan=3)

spanish_characters = ["ñ", "á", "é", "í", "ó", "ú"]
for index, char in enumerate(spanish_characters):
    bouton_char = tk.Button(frame_caracteres, text=char, command=lambda c=char: ajouter_caractere(c))
    bouton_char.grid(row=3, column=index)

# Commencer par poser une première question
nouvelle_question()

# Lancer l'application
app.mainloop()
