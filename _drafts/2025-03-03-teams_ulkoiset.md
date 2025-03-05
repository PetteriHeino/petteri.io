---
title: Teams ja ulkoiset käyttäjät
description: >-
 Teams käyttöön vaikuttavat M365 tenantin yhteistyö asetukset ulkoisten käyttäjien kanssa ovat hujan hajan M365 palvelussa. Tässä kirjoituksessa pyrin niputtamaan nämä yhteen dokumenttiin ja käyn läpi niiden konfiguroinnin. Näitä asioita testataan vuoren varmasti MS-700 seritifiontitestissä.  
authors: Petteri
date: 2025-03-02 12:00:00 +0200
categories: [M365]
tags: [Teams]
pin: false
media_subpath: '/assets/media/2025-03-03-teams_ulkoiset'
published: false
---
## Johdanto

Kun aloittaa opiskelemaan Teamsiä sertifioinnin suorattamista ajatellen, eikä kokemusta Teams ylläpidosta ole entuudestaan, niin ulkoisten käyttäjien asetusten määrityksiin voi olla alkuun hankala saada otetta, kun on useampaa erilaista kollaboraatio tyyppiä ja asetuksia on M365n eri palveluissa.

Kollaboraatio ulkoisten käyttäjien kanssa on aihealua, mitä sertifiointi testissä kysytään varmasti, niin ajattelin yhdistää nämä eri kollaboraatio asetukset yhteen dokumenttiin.

## Lyhyesti koko paletti

- External Access
    - Tenantin käyttäjät voivat chatata ulkoisten käyttäjien kanssa
    - Ulkoisilla käyttäjillä ei ole pääsyä tenantin tiimeihin ja resursseihin
    - Asetukset: Teams hallintakeskus Users -> External Access -> Organization settings ja Policies
- Guest Access
    - vieraskäyttäjille voidaan antaa pääsy Teamsiin, kanavien dokumentteihin, chatiin ja sovelluksiin
    - Asetukset: 
        - Entra ID -> External collaboration settings
        - M365 admin center -> Settings -> M65 Groups
        - Sharepoint admin center -> Policies -> Sharing
        - Teams Admin Center -> Users -> Guest access
- B2B Direct Connect ja Shared Channel
    - Luottosuhde tenantien välillä mahdollistaa shared channel käytön, missä kollaboraatioon ei tarvitse guest tunnusta ja näin ollen ulkoisen käyttäjän ei tarvitse kirjautua tenantiin, vaan hän käytää Teams kanavaa oman tenanttinsa tunnuksella.
    - Asetukset:
        - Entra ID -> 
        - Teams Admin Center ->
        - Teams client ->






## Shared Channel - B2B luottosuhde ja Teams ryhmän shared channel 

### B2B direct connect konfiguraatiot (Entra ID)

#### Lisää partner tenant

To configure cross-tenant access settings in the Microsoft Entra admin center, you need an account with at least the Security Administrator role. Teams administrators can read cross-tenant access settings, but they can't update these settings.




![Entra ID polku](/crosstenant_access_settings.png)

Sign in to the Microsoft Entra admin center as at least a Security Administrator.

Browse to Identity > External Identities > Cross-tenant access settings.

Select Organizational settings.

Select Add organization.

On the Add organization pane, type the full domain name (or tenant ID) for the organization.


![add domain](/crosstenant_add_domain.png)


Select the organization in the search results, and then select Add.

The organization appears in the Organizational settings list. At this point, all access settings for this organization are inherited from your default settings. To change the settings for this organization, select the Inherited from default link under the Inbound access or Outbound access column.

![added domain](/crosstenant_added_domain.png)

#### Määritä pääsyoikeudet

If you're configuring settings for an organization, select one of these options:

Default settings: The organization uses the settings configured on the Default settings tab. If customized settings were already configured for this organization, you need to select Yes to confirm that you want all settings to be replaced by the default settings. Then select Save, and skip the rest of the steps in this procedure.

Customize settings: You can customize the settings to enforce for this organization instead of the default settings. Continue with the rest of the steps in this procedure.

Select External users and groups.

Under Access status, select one of these options:

Allow access: Allows the users and groups specified under Applies to to access B2B direct connect.
Block access: Blocks the users and groups specified under Applies to from accessing B2B direct connect. Blocking access for all external users and groups also blocks all your internal applications from being shared via B2B direct connect.

If you chose Select external users and groups, do the following for each user or group you want to add:

Select Add external users and groups.
In the Add other users and groups pane, type the user object ID or the group object ID in the search box.
In the menu next to the search box, choose either user or group.
Select Add.

![Allow access to groups or/and users](/custom_allow_access.png)

![Allow access to M365 application](/custom_allow_access2.png)

#### partner organisaation luottosuhde - outbound access

In the Microsoft Entra admin center, select External Identities, and then select Cross-tenant access settings.
Select the outbound access link for the organization that you want to modify.
On the B2B direct connect tab, choose Customize settings.
On the External users and groups tab, choose Allow access and set an Applies to of all users.
On the External applications tab, choose Allow access and Select external applications.
Select Add Microsoft applications.
Select the Office 365 application, and then choose Select.
Select Save, choose Yes to confirm, and close the Outbound access settings blade.

### Teams shared channel (Teams hallintakonsoli tai Teams client)

