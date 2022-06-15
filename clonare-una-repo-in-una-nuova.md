1. creare la nuova repo
2. clonare la repo vecchia
3. eliminare la cartella .git dalla cartella della repo riclonata
4. git init
5. git add -A
6. git commit -m 'first commit'
7. git remote add origin [url della repo nuova]
8. git branch -M main
9. git push -u origin main
10. fare il .env e tutte le operazioni base

Obbiettivo di oggi: terminare le CRUD dei posts e creare una nuova entitità nel DB con una relazione one-to-many con l'entità posts