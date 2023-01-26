# UMSA Galaxy

Endpoint: https://umsa.cerit-sc.cz/

Předpisy pro deployment (Ansible): https://gitlab.ics.muni.cz/recetox/mass-spectrometry/umsa-galaxy

Definice nástrojů: https://github.com/RECETOX/galaxytools

Zdrojáky vlastních nástrojů (a forků): https://github.com/RECETOX/ 

## Virtuální cluster

Openstack, projekt egi_eu/umsa.cerit-sc.cz
- původně deployment přes IM, postupně se redukoval natolik, že přestal mít smysl
- v tomto projektu, navázaném na VO, zůstalo kvůli vykazování spotřeby zdrojů v EOSC-Synergy, zřejmě lze časem opustit
- deployment ručně v OS, flavory podle https://gitlab.ics.muni.cz/recetox/mass-spectrometry/umsa-galaxy/README.md : 
  - vnode-0: frontend Galaxy, uwsgi jako webový interface  
  - vnode-1,2,3: malé workernody (8 jader, 32 GB RAM)
  - vnode-4: tlustý workernode pro speciální joby (32 jader, 256 GB RAM, GPU)

## Deployment Galaxy

Ansible
- předpisy v https://gitlab.ics.muni.cz/recetox/mass-spectrometry/umsa-galaxy
- known bug: funguje jen do verze 5
- používá velké kusy zjevně kopírované z https://galaxy.ansible.com, ale těžko říct, jak moc změněné

Webové rozhraní
- uwsgi, de facto default Galaxy
- certifikát Let's Encrypt
- AFAIK žádná speciální konfigurace

## Data

Sdílený filesystém Galaxy
- /mnt/volume/share/galaxy, NFS mount z frontendu (vnode-0)

Ces-nya (Lukášovo SSD pole)
- mount na /mnt/volume/shared/ces-nya/ na všech uzlech
- vše v nfs4/home/umsa, autentizace kerberem, periodicky obnovovaný umsa@EINFRA z ~galaxy/umsa.keytab
- používá se jako sdílený pracovní adresář jobů a cache pro soubory z objektového úložiště; podadresáře:
   - cache/object_store_cache/
   - job_working_directory_object/
   - tmp/

Objektové úložiště
- permanentní uložení Galaxy datasetů (spoléháme, že Antoš zálohuje)
- přístup přes s3.cl1.du.cesnet.cz
- dedikovaný access key (viz config/object_store_conf.xml)

Experimentální data
- mount ze //sally.hobit.cetoc.muni.cz/home (CIFS) na /mnt/sally
- v Galaxy jako shared data library 

Databáze 
- PostgreSQL, běží na frontentendu
- pravidelné dumpy do db/ na ces-nya:... home/umsa
- kvůli objektovému uložišti extrémně citlivá, bez ní už nikdy nedohledáme, co je co


## Uživatelé

AuthN Elixir/LifeScience AAI

AuthZ AFAIK zatím žádná

De facto jen velmi úzký okruh lidí z Recetoxu a bezprostředně spolupracujících


## Joby

Dedikovaná instalace SLURM, server na frontendu

Instalace a Ansible, v https://gitlab.ics.muni.cz/recetox/mass-spectrometry/umsa-galaxy 
je na to z nějakých důvodů extra playbook

## Nástroje

Specifická sada pro potřeby komunity, částečně standardní, sada vlastních 

Definice pro Galaxy v https://github.com/RECETOX/galaxytools

TODO: není mi zcela zřejmý mechanismus konfigurace

Integrační testy v Github Actions

Zdrojáky vlastních nástrojů a forků v https://github.com/RECETOX/

## Workflow

https://github.com/RECETOX/workflow-testing

ale nevypadá to příliš živě, a neodpovídá zveřejněným workflow přímo v umsa.cerit-sc.cz; i tam je ale dost chaos
