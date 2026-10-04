# ERP Transport --- Journal des décisions

  -----------------------------------------------------------------------
  ID                      Décision                Statut
  ----------------------- ----------------------- -----------------------
  D-001                   SaaS multi-tenant avec  VALIDÉ
                          PostgreSQL partagé      

  D-002                   Isolation par tenant_id VALIDÉ
                          et RLS                  

  D-003                   Le voyage est une       VALIDÉ
                          occurrence d'un trajet  

  D-004                   Le trajet est orienté   VALIDÉ
                          départ → arrivée        

  D-005                   Voyages NORMAL ou       VALIDÉ
                          NAVETTE                 

  D-006                   NORMAL = PROGRAMMÉ ou   VALIDÉ
                          NON_PROGRAMMÉ           

  D-007                   NAVETTE = colis         VALIDÉ
                          uniquement              

  D-008                   Réservation uniquement  VALIDÉ
                          sur PLANIFIÉ            

  D-009                   Affectation du bus →    VALIDÉ
                          VENTE OUVERTE           

  D-010                   PLEIN si billets VALIDÉ VALIDÉ
                          = capacité              

  D-011                   Réservations EN_COURS   VALIDÉ
                          exclues du calcul PLEIN 

  D-012                   Billets : VALIDÉ /      VALIDÉ
                          ANNULÉ / REMBOURSÉ      

  D-013                   Pas de suppression      VALIDÉ
                          physique des billets    

  D-014                   Tout colis est rattaché VALIDÉ
                          à un voyage             

  D-015                   Les colis suivent       VALIDÉ
                          automatiquement les     
                          transitions EN_ROUTE /  
                          ARRIVÉ du voyage        

  D-016                   Statuts colis           À FINALISER
                          définitifs à harmoniser 

  D-017                   Permissions définies    VALIDÉ
                          par le produit et       
                          immuables               

  D-018                   Rôles configurés par    VALIDÉ
                          regroupement de         
                          permissions             

  D-019                   OWNER/ADMIN ne          VALIDÉ
                          réalisent pas les       
                          opérations quotidiennes 

  D-020                   Création agence/bus par VALIDÉ
                          ADMIN soumise à         
                          validation OWNER        
  -----------------------------------------------------------------------

Toute modification d'une décision doit être explicitement validée et
consignée ici.
