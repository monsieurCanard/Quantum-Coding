<div align="center">
<h1>🌌 Ftl-Quantum </h1>
  <h2>Introduction et Cours Pratique sur l'Informatique Quantique</h2>
  <p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Qiskit-6929C4?style=for-the-badge&logo=qiskit&logoColor=white" alt="Qiskit">
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=Jupyter&logoColor=white" alt="Jupyter">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Active-success.svg?style=flat-square" alt="Status">
</p>
</div>

Ce projet est une initiation pratique et, j'espère, accessible. Personellement, j'ai toujours été intrigué par la physique quantique mais coder "quantiquement" me semblait être un défi de taille. J'ai voulu créer une série de notebooks qui démystifient les concepts clés de l'informatique quantique, tout en vous permettant de les expérimenter par vous-même. Je sais que c'est pas vraiment "intuitif" au début, mais c'est justement pour ça que j'ai voulu faire ce projet : pour apprendre en faisant, et pour partager cette expérience avec d'autres curieux.

À travers plusieurs notebooks interactifs, vous allez manipuler de vrais qubits grâce à la bibliothèque Python **Qiskit**.
> [!NOTE]
> J'ai utilisé Qiskit 1.x alors que la version 2.x est sortie récemment. Pourquoi ? Parce que la documentation et les ressources en ligne sont encore largement basées sur la 1.x, et je voulais que ce projet soit le plus accessible possible pour moi sachant que je pars de zéro.

## 📁 Au programme

### 1. La Superposition (1-superposition)

> [!NOTE]
> **En mots simples :** Un bit normal (comme ceux de votre ordinateur classique) vaut soit `0`, soit `1`. Un qubit en superposition, lui, c'est comme une pièce de monnaie qui tourne en l'air : il est un mélange de `0` et de `1` en même temps, jusqu'à ce qu'on "l'arrête" pour le regarder (la mesure).

**Un peu de maths :** L'état s'écrit $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$. Les $\alpha$ et $\beta$ sont simplement les probabilités (amplitudes) d'obtenir l'un ou l'autre à la fin.

**🖥️ En Qiskit :**
```python
qc = QuantumCircuit(1)
qc.h(0) # La porte de Hadamard (H) met le qubit en superposition
qc.measure_all() # On regarde le résultat !
```
**📷 Circuit:**
<div align="center">
<img width="338" height="174" alt="Image" src="https://github.com/user-attachments/assets/f81e1b59-f18c-4e9c-93b2-445568b2a59d" />
</div>

**📊 Resultats:**
<div align="center">
  <img width="630" height="470" alt="Image" src="https://github.com/user-attachments/assets/e49f2295-4cc8-452a-b7c2-c40b181e5dfd" />
</div>

**🔮Representation Sphere de Bloch:**
<div align="center">
  <img width="438" height="454" alt="Image" src="https://github.com/user-attachments/assets/9ed03f1b-b090-4d5c-bb70-3fd3549ee2a5" />
</div>
> [!NOTE]
> **🤔 Question : Comment on "visualise" un qubit ?**  
> Comme un qubit est un mélange continu (des probabilités) de `0` et de `1`, on le représente en 3D sur ce qu'on appelle la **Sphère de Bloch**. Imaginez un globe terrestre : le **Pôle Nord** représente `0`, et le **Pôle Sud** représente `1`. Quand on met le qubit en superposition (grâce à la porte `H`), sa flèche quitte le pôle pour pointer pile sur **l'équateur** ! *(Vous le verrez en action dans le notebook 1).*

> [!NOTE]
> **🤔 Question : Que se passe-t-il avec DEUX portes de Hadamard d'affilée (`qc.h(0)` puis encore `qc.h(0)`) ?**  
> Intuitivement, on pourrait se dire "ça double le hasard". Faux ! La quantique est géométrique et **réversible**. Si vous lancez une pièce pour la faire tourner (1ère porte `H`), et que vous appliquez *exactement* le même mouvement (2ème porte `H`), la pièce s'arrête net sur sa face de départ. Donc `H` + `H` = Annulation : le qubit redevient simplement `0`. Rien n'est détruit, tout se transforme.

---

### 2. L'Intrication Quantique (2-entanglement)

> [!IMPORTANT]
> **En mots simples :** C'est ce qu'Einstein appelait une "action fantôme à distance". Prenez deux qubits et intriquez-les. Si vous lisez la valeur du premier et tombez sur `0`, le second vaudra INSTANTANÉMENT `0` aussi, même s'il se trouve sur la planète Mars. Ils sont magiquement connectés !

Pour créer cette intrication, on utilise souvent une **porte CNOT (ou cx)**. Elle lit un qubit "contrôle" et inverse le qubit "cible" uniquement si le contrôle vaut 1 :

| 🎮 Contrôle (Entrée) | 🎯 Cible (Entrée) | ➔ | 🎮 Contrôle (Sortie) | 🎯 Cible (Sortie) | Explication |
| :---: | :---: | :---: | :---: | :---: | :--- |
| `0` | `0` | ➔ | `0` | `0` | Le contrôle est 0, on ne touche à rien. |
| `0` | `1` | ➔ | `0` | `1` | Le contrôle est 0, on ne touche à rien. |
| `1` | `0` | ➔ | `1` | `1` | Le contrôle est 1, la cible **s'inverse**. |
| `1` | `1` | ➔ | `1` | `0` | Le contrôle est 1, la cible **s'inverse**. |

**🖥️ En Qiskit :**
```python
qc = QuantumCircuit(2)
qc.h(0)     # On met le premier qubit en superposition (il est un mix de 0 et 1)
qc.cx(0, 1) # La porte CNOT lie le qubit 1 au comportement du qubit 0
qc.measure_all()
```

> [!TIP]
> **🎯 Résultat attendu :** Uniquement des paires identiques (puisque la cible copie ou suit le contrôle) : environ 50% de `00` et 50% de `11`. Jamais de `01` ou de `10`.

---

### 3. Le Bruit Quantique (Le monde physique) (3-quantum_noise)

> [!WARNING]
> **En mots simples :** Sur simulateur, tout est beau et parfait. Mais dans la vraie vie, les puces quantiques actuelles (qu'on appelle NISQ) sont fragiles. Elles subissent des interférences parasites de leur environnement (le "bruit quantique"), un peu comme des parasites sur une vieille radio.

**Pourquoi s'en soucier ?** 
Dans ce notebook, on envoie *vraiment* notre code s'exécuter sur les ordinateurs quantiques physiques d'IBM. On réalise qu'un circuit censé renvoyer uniquement `00` et `11` va, à cause du bruit, cracher accidentellement quelques `01` ou `10`. Un formidable défi de l'ingénierie moderne !

> [!NOTE]
> **🤔 Question : Pourquoi s'embêter avec ces machines d'IBM "bruyantes" si le simulateur (PC classique) est si parfait ?**  
> Parce qu'on ne peut pas le simuler éternellement ! Pour simuler l'état d'un système quantique, la mémoire nécessaire grandit de façon **exponentielle** ($2^N$). Pour 10 qubits, c'est 1024 valeurs. Pour 50 qubits, cela sature complètement la RAM des plus gigantesques supercalculateurs mondiaux. Et pour seulement 300 qubits, il faudrait stocker plus de valeurs... qu'il n'y a d'atomes dans l'univers observable ! C'est ce "mur de la complexité classique" qui nous oblige à construire et utiliser de vraies puces quantiques, même si elles ont encore du bruit.

---

### 4. L'Algorithme de Deutsch-Jozsa (4-deutsch-jozsa)

> [!NOTE]
> **En mots simples :** Imaginez un distributeur de canard mystère : donne-t-il toujours le même canard (goût "Constant"), ou bien un mix équitable de deux goûts ("Balancé") ?
> 
> Avec un ordinateur normal, vous devez acheter *plusieurs* canards pour en être sûr. L'algorithme quantique, lui, permet de le savoir du **tout premier coup** grâce aux ondes quantiques qui s'annulent entre elles !

**🖥️ En Qiskit (un des oracles possibles) :**
```python
qc = QuantumCircuit(4, 3) # Un exemple pour 3 qubits + 1 qubit de travail
qc.x(3) # Initialisation
qc.h([0,1,2,3])
# -- Ajout de l'oracle ici --
qc.h([0,1,2])
qc.measure([0,1,2], [0,1,2])
```

---

### 5. L'art de chercher vite : Algorithme de Grover (search_algo)

> [!NOTE]
> **En mots simples :** C'est comment chercher une aiguille dans une botte de foin. Si vous cherchez une combinaison secrète parmi un million de possibilités, un PC normal testera en moyenne 500 000 fois. L'algorithme de Grover amplifie la "bonne réponse" à chaque essai, réduisant le nombre de tests nécessaires à seulement 1 000 !

**🖥️ En Qiskit (l'Oracle) :**
```python
oracle = QuantumCircuit(3)
oracle.h(2)
oracle.ccx(0, 1, 2) # L'oracle cible l'état secret (par exemple "111")
oracle.h(2)
# Ensuite, on applique le "diffuseur" de Grover pour amplifier cette réponse.
# Pour savoir combien de fois répéter, on utilise la formule : nombre d'itérations ≈ π/4 * √N (N = nombre de possibilités).
```

> [!TIP]
> **🎯 Résultat attendu :** Un pic quasi-parfait ! Si l'élément secret est `111`, l'histogramme de résultats renverra cet état avec une probabilité bien plus élevée que les autres.

## 🚀 Installation & Exécution

Pour utiliser ces jupyter notebooks, vous devez d'abord comprendre python et son environment virtuel. Je vous recommande d'utiliser `venv` ou `conda` pour créer un environnement isolé. Ensuite, installez les dépendances nécessaires avec la commande suivante :

```bash
pip install qiskit qiskit[visualization] qiskit-aer qiskit-ibm-runtime
```

> [!IMPORTANT]
> Pour utiliser le vrai hardware IBM Quantum sur le notebook `3-quantum_noise`, pensez à définir votre jeton `IBM_QUANTUM_KEY` dans vos variables d'environnement.
