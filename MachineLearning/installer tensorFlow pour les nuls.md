#Installer tensorFlow Windows pour les nuls.
##Intro
Bon a priori, si vous lisez ceci, c'est que vous savez déjà ce qu'est tensorFlow, la lib de google dédiée entre autres au deep learning.
Pour plus d'infos: https://www.tensorflow.org/
Si a priori j'ai décidé de faire ce tuto, c'est que j'ai moi même pas mal galéré avant de réussir à obtenir une version vraiment fonctionnelle avec ma config. 
Du coup, je vais essayer d'être le plus clair possible, et de détailler les différents types d'installations possibles. 

## GPU or not GPU? 
Il existe deux installations possibles pour tensorflow. L'installation classique, et l'installation gpu. L'installation GPU permet d'utiliser la puissance de calcul de votre carte graphique ce qui permet d'améliorer grandement les temps de processing des données. 
>Toutefois tous les ordis ne peuvent pas installer cette version: 
Vérifiez que votre carte graphique est dans cette liste, et possède au moins une capacité de computation de 3: https://developer.nvidia.com/cuda-gpus

Si vous choisissez d'installer la version classique, vous pouvez directement passer à la section suivante. 

### Prérequis à l'installation GPU
Pour pouvoir pleinement profiter de la puissance que propose la version GPU de tensorFlow, il vous faut les outils NVDIA associés: 
- Le CUDA toolkit v8: https://developer.nvidia.com/cuda-downloads
- Le CudaNN toolkit, utile pour le deep learning: https://developer.nvidia.com/cudnn (note: il faut être enregistré pour pouvoir le télécharger).

Installez donc le CUDA toolkit, et téléchargez le CudaNN toolkit, ce dernier est sous forme d'archive, l'extraire et repérer le path où se trouve le dll. 
Par exemple pour moi: 
```
C:\Users\Orion\Documents\Tools\cuda\bin 
```
Ajoutez ensuite ce chemin à votre variable d'environnement %PATH% comme ceci: 

```
set PATH=%PATH%;C:\Users\Orion\Documents\Tools\cuda\bin 
```

Voilà, maintenant le plus dur est fait! 

## Je n'ai pas encore d'installation Python: 
Si jamais vous n'avez pas encore Python installé sur votre ordinateur, il faut installer la version 3.5.2, la version 3.6 n'étant pas encore supportée par tensorFlow sur Windows.
  Ca se passe ici: https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe
## Installation de tensorFlow
### J'ai un interpreteur Python classique: 
Vérifiez seulement que vous tournez bien en 3.5.2, autrement tensorFlow ne sera pas compatible. 
#### tensorFlow classique: 
Un simple : 
```
pip3 install --upgrade tensorflow 
```
Et voilà c'est fini!
#### tensorFlow GPU: 
>A l'heure où j'écris cet article l'installation recommandée sur le site officiel contient des bugs, il est possible que dans le futur cette commande suffise: 
```
pip3 install --upgrade tensorflow-gpu
```
- aller sur http://ci.tensorflow.org/view/Nightly/job/nightly-win/
- choisir la version 85 (ou tentez une version stable plus actuelle si le coeur vous en dit) 
- choisir la version gpu
- télécharger le fichier .whl

puis: 
```
pip3 install nom_du_fichier.whl
```

Et voilà c'est fini!
### J'ai un interpreteur Anaconda:
C'est plus compliqué, sachant que tensorFlow v1.0 n'est pas encore disponible de façon officielle pour Anaconda. 
un conda install ne marche donc pas. 
Tout d'abord, Vérifiez que votre version d'Anaconda est au moins 3.5, si elle ne l'est pas
```
conda install python = 3.5.2
```
Il s'agit ensuite de créer un environnement virtuel: 
```
conda create env -n tensorflow
activate tensorflow
```
Votre prompt devrait alors changer et afficher (tensorflow) avant le path. 
Entrez alors cette commande pour la version non gpu:
```
pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/windows/cpu/tensorflow-1.0.0-cp35-cp35m-win_x86_64.whl
```
Pour la version gpu:
```
pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-1.0.0-cp35-cp35m-win_x86_64.whl
```
> note:  à l'heure où je rédige cet article la dernière version gpu contient encore des erreurs, je recommande donc de télécharger le fichier wheel à cette adresse: http://ci.tensorflow.org/view/Nightly/job/nightly-win/85/DEVICE=gpu,OS=windows/artifact/cmake_build/tf_python/dist/tensorflow_gpu-1.0.0rc2-cp35-cp35m-win_amd64.whl
et de procéder à un:
```
pip install nom_du_fichier.whl
```
**La contrainte principale avec Anaconda est de devoir pour le moment travailler avec un environnement virtuel pour faire tourner tensorFlow**

##Test de l'installation et optimisation:

lancez votre interpreteur python avec 
```
python
```
Puis entrez les commandes suivantes: 
```
import tensorflow as tf

hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```
Votre terminal devrait afficher b'Hello, TensorFlow!'
> **Note** Il est fort probable que de nombreux autres messages s'affichent: certains commencent, par un I, d'autres un W, et d'autres encore par un E.
Ils correspondent à Info, Warning et Error. 
Pour ceux utilisant la version GPU, vérifiez dans les Infos que toutes les librairies sont correctement ouvertes. Si ce n'est pas le cas, vérifiez que vous avez bien ajouté le cudann au path.
Si il y a des Erreurs, essayez d'installer une version stable plus récente sur http://ci.tensorflow.org/view/Nightly/job/nightly-win/.
**Enfin, si comme moi vous ne voulez pas forcément avoir tous ces messages d'infos à chaque fois que vous lancez un build: **
Rendez vous dans vos variables d'environnement et ajoutez la variable : **TF_CPP_MIN_LOG_LEVEL** et donnez lui la valeur: 
- 0: pour avoir warnings, infos et erreurs
- 1: pour ne plus avoir les infos
- 2: pour ne plus avoir ni les infos ni les warnings
- 3: pour ne plus avoir ni infos, ni warnings, ni erreurs (Déconseillé)

Voilà, c'est tout, bonne chance et n'hésitez pas à faire des remarques, poser des questions ou des updates.
