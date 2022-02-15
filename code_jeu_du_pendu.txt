import random
HANGMAN_PICS = ['''
  +---+
      |
      |
      |
     ===''', '''
  +---+
  O   |
      |
      |
     ===''', '''
  +---+
  O   |
  |   |
      |
     ===''', '''
  +---+
  O   |
 /|   |
      |
     ===''', '''
  +---+
  O   |
 /|\  |
      |
     ===''', '''
  +---+
  O   |
 /|\  |
 /    |
     ===''', '''
  +---+
  O   |
 /|\  |
 / \  |
     ===''', '''
  +---+
 [O   |
 /|\  |
 / \  |
     ===''', '''
  +---+
 [O]  |
 /|\  |
 / \  |
     ===''']
words = {'Couleurs':'blanc bleu indigo jaune marron noir orange rouge vert violet'.split(),
'Formes':'cercle chevron ellipse heptagone hexagone octogone pentagone rectangle triangle '.split(),
'Fruits':'banane cassis cerise citron framboise orange mangue melon pamplemousse poire pomme raisin'.split(),
'Animaux':'aigle babouin baleine belette blaireau bouc canard canari castor cerf chameau chat cheval chien chouette cigogne cobra cochon corbeau corneille couleuvre coyote crapaud crotale cygne dauphin dindon dromadaire faucon fourmi furet gorille grenouille hibou hippopotame lama lapin lion loup mouton mule mygale oie otaries ours palourdes panda paresseux perroquet phoque pigeon poisson puma putois python rat renard  renne requin salamandre saumon singe souris taupe tigre tortue truite '.split()}

def getRandomWord(wordDict):
    # Retourne un mot au hasard parmi ceux d'une liste.
    # Sélectionne d'abord aléatoirement la clé.
    wordKey = random.choice(list(wordDict.keys()))
    # Sélectionne ensuite un mot de la liste correspondante.
    wordIndex = random.randint(0, len(wordDict[wordKey]) - 1)

    return [wordDict[wordKey][wordIndex], wordKey]

def displayBoard(missedLetters, correctLetters, secretWord):
    print(HANGMAN_PICS[len(missedLetters)])
    print()

    print('Mauvaises lettres:', end=' ')
    for letter in missedLetters:
        print(letter, end=' ')
    print()

    blanks = '_' * len(secretWord)
    # Remplace les tirets par les lettres correctements devinées.
    for i in range(len(secretWord)):
        if secretWord[i] in correctLetters:
            blanks = blanks[:i] + secretWord[i] + blanks[i+1:]
    # Affiche le mot secret avec des espaces entre les lettres.
    for letter in blanks:
        print(letter, end=' ')
    print()

def getGuess(alreadyGuessed):
    # Affiche la lettre saisie par le joueur. S'assure qu'il sagit d'une unique lettre et de rien d'autre.
    while True:
        print('Propose une lettre.')
        guess = input()
        guess = guess.lower()
        if len(guess) != 1:
            print('Propose une seule lettre STP.')
        elif guess in alreadyGuessed:
            print('Tu as déjà demandé cette lettre.')
        elif guess not in 'abcdefghijklmnopqrstuvwxyz':
            print('Propose une LETTRE STP.')
        else:
            return guess

def playAgain():
    # Retourne True si le joueur veut recommencer, False sinon.
    print('Veux-tu rejouer? (oui ou non)')
    return input().lower().startswith('o')


print('P E N D U')

difficulty = ' '
while difficulty not in 'FDM':
  print('Choisir le niveau : F - Facile, M - Moyen, D - Difficile')
  difficulty = input().upper()
if difficulty == 'M':
    del HANGMAN_PICS[8]
    del HANGMAN_PICS[7]
if difficulty == 'D':
    del HANGMAN_PICS[8]
    del HANGMAN_PICS[7]
    del HANGMAN_PICS[5]
    del HANGMAN_PICS[3]

missedLetters = ''
correctLetters = ''
secretWord, secretSet = getRandomWord(words)
gameIsDone = False

while True:
    print('Le mot secret est dans la catégorie : ' + secretSet)
    displayBoard(missedLetters, correctLetters, secretWord)

    # Invite le joueur à proposer une lettre.
    guess = getGuess(missedLetters + correctLetters)

    if guess in secretWord:
        correctLetters = correctLetters + guess

        # Vérifie si le joueur a gagné.
        foundAllLetters = True
        for i in range(len(secretWord)):
            if secretWord[i] not in correctLetters:
                foundAllLetters = False
                break
        if foundAllLetters:
            print('Oui! Le mot secret est "' + secretWord + '"! Tu as gagné !')
            gameIsDone = True
    else:
        missedLetters = missedLetters + guess

        # Vérifie si le joueur a perdu.
        if len(missedLetters) == len(HANGMAN_PICS) - 1:
            displayBoard(missedLetters, correctLetters, secretWord)
            print('Tu as épuisé tous tes essais!\nAprès ' + str(len(missedLetters)) + ' mauvaises lettres et ' + str(len(correctLetters)) + ' lettres exactes, le mot secret était "' + secretWord + '"')
            gameIsDone = True

    # Demande au joueur s'il veut recommencer (seulement si la partie est finie).
    if gameIsDone:
        if playAgain():
            missedLetters = ''
            correctLetters = ''
            gameIsDone = False
            secretWord, secretSet = getRandomWord(words)
        else:
            break