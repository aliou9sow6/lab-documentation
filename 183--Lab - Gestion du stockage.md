

aws ec2 stop-instances --instance-ids INSTANCE-ID





echo "\* \* \* \* \*  aws ec2 create-snapshot --volume-id vol-049a2d8cda49d3595 2>\&1 >> /tmp/cronlog" > cronjob

crontab cronjob



aws ec2 describe-snapshots --filters "Name=volume-id,Values=vol-049a2d8cda49d3595"





\[ec2-user@ip-10-5-0-250 \~]$ aws ec2 describe-snapshots --filters "Name=volume-id, Values=vol-049a2d8cda49d3595" --query 'Snapshots\[\*].SnapshotId'

\[

&#x20;   "snap-0a6c1da882c9bd4d0",

&#x20;   "snap-01016a6a1191366d1"

]



\[ec2-user@ip-10-5-0-250 \~]$ wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-RSJAWS-3-124627/183-lab-JAWS-managing-storage/s3/files.zip



\[ec2-user@ip-10-5-0-250 \~]$ unzip files.zip



s3-bucket-anoors



###### **-- DEFI 1**



* Activer la gestion des versions pour votre compartiment Amazon S3.



aws s3api put-bucket-versioning --bucket s3-bucket-anoors --versioning-configuration Status=Enabled



* Utiliser une seule commande de l'AWS CLI pour synchroniser le contenu de votre dossier décompressé avec votre compartiment Amazon S3.



aws s3 sync files s3://s3-bucket-anoors/files/



* Modifier la commande afin qu'elle supprime un fichier d'Amazon S3 lorsque le fichier correspondant est supprimé localement sur votre instance.



rm files/file1.txt



* Récupérer le fichier supprimé d'Amazon S3 à l'aide de la gestion des versions.



aws s3api list-object-versions --bucket s3-bucket-anoors --prefix files/file1.txt





Pour supprimer le même fichier du compartiment S3, utilisez l'option --delete avec la commande aws s3 sync. 



Exécutez la commande suivante en remplaçant « S3-BUCKET-NAME » par le nom de votre compartiment :



aws s3 sync files s3://s3-bucket-anoors/files/ --delete





Pour vérifier que le fichier a été supprimé du compartiment, exécutez la commande suivante en remplaçant « S3-BUCKET-NAME » par le nom de votre compartiment :



aws s3 ls s3://s3-bucket-anoors/files/





Maintenant, essayez de récupérer l'ancienne version du fichier file1.txt. Pour afficher la liste des versions précédentes de ce fichier, exécutez la commande suivante en remplaçant « S3-BUCKET-NAME » par le nom de votre compartiment :



aws s3api list-object-versions --bucket s3-bucket-anoors --prefix files/file1.txt





Étant donné qu'il n'existe aucune commande directe pour restaurer une ancienne version d'un objet Amazon S3 dans son propre compartiment, vous devez télécharger et synchroniser à nouveau l'ancienne version dans Amazon S3. 



Pour télécharger la version précédente du fichier file1.txt, exécutez la commande suivante en remplaçant « S3-BUCKET-NAME » par le nom de votre compartiment :



aws s3api get-object --bucket s3-bucket-anoors --key files/file1.txt --version-id Qm0QMLLG.uyBrxhHUCNMBdi7yT\_arQRu files/file1.txt





Pour vérifier que le fichier a été restauré localement, exécutez la commande suivante :



ls files



La commande affiche les trois fichiers répertoriés.



Pour synchroniser à nouveau le contenu du dossier files/ vers Amazon S3, exécutez la commande suivante sur votre instance en remplaçant « S3-BUCKET-NAME » par le nom de votre compartiment :



aws s3 sync files s3://s3-bucket-anoors/files/



Enfin, pour vérifier qu'une nouvelle version de file1.txt a été transférée (push) vers le compartiment S3, exécutez la commande suivante en remplaçant « S3-BUCKET-NAME » par le nom de votre compartiment :



aws s3 ls s3://s3-bucket-anoors/files/





* ###### Conclusion





Créer et gérer des instantanés pour les instances Amazon EC2.



Utiliser Amazon S3 sync pour copier des fichiers d'un volume EBS vers un compartiment S3.



Utiliser la gestion des versions Amazon S3 pour récupérer les fichiers supprimés.







