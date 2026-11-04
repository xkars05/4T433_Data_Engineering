# 📘 Návod pro práci s repozitářem
 
Tento dokument je určen všem členům týmů. Přečti si ho celý dřív, než začneš pracovat.
 
---
 
## 📋 Obsah
 
1. [Co je GitHub a jak funguje](#1-co-je-github-a-jak-funguje)
2. [Základní pojmy](#2-základní-pojmy)
3. [Jak vytvořit týmový repozitář a dostat přístup](#3-jak-vytvořit-týmový-repozitář-a-dostat-přístup)
4. [Jak nastavit CODEOWNERS, ochranu větve main a větev dev](#4-jak-nastavit-codeowners-ochranu-větve-main-a-větev-dev)
5. [Jak propojit Fabric workspace s repozitářem](#5-jak-propojit-fabric-workspace-s-repozitářem)
6. [Jak upravit soubor přímo na GitHubu](#6-jak-upravit-soubor-přímo-na-githubu)
7. [Jak vytvořit Pull Request z dev do main](#7-jak-vytvořit-pull-request-z-dev-do-main)
8. [Jak schválit Pull Request](#8-jak-schválit-pull-request)
9. [Jak označit odevzdaný stav](#9-jak-označit-odevzdaný-stav)
10. [Pravidla repozitáře](#10-pravidla-repozitáře)
11. [Časté chyby a jak je opravit](#11-časté-chyby-a-jak-je-opravit)
---
 
## 1. Co je GitHub a jak funguje
 
GitHub je platforma pro sdílení a správu souborů. Funguje jako sdílená složka, ale s jedním velkým rozdílem: **u každé změny je zaznamenáno, kdo ji udělal a kdy**. Všechno je verzované, takže se vždy můžeš vrátit ke starší verzi.
 
### Co se do GitHubu ukládá a co ne
 
Fabric do repozitáře ukládá **definice položek**: kód notebooků, nastavení pipeline a Copy jobu, metadata lakehouse.
 
**Data se do GitHubu neukládají.** Tabulky v lakehouse na GitHubu neuvidíš a je to tak správně. Repozitář obsahuje návod, jak data vytvořit, ne data samotná.
 
---
 
## 2. Základní pojmy
 
| Pojem | Vysvětlení |
|---|---|
| **Repozitář (repo)** | Složka se všemi soubory projektu |
| **Branch (větev)** | Samostatná linie změn v repozitáři |
| **`main`** | Hlavní větev se schváleným stavem. Mění se jen přes Pull Request |
| **`dev`** | Pracovní větev týmu. Sem commituje Fabric workspace DEV |
| **Commit** | Uložení změn s popisem, co se změnilo |
| **Pull Request (PR)** | Žádost o převzetí změn z `dev` do `main`, kterou schvaluje jiný člen týmu |
| **Merge** | Sloučení změn z `dev` do `main` po schválení PR |
| **CODEOWNERS** | Soubor, který určuje, kdo schvaluje změny v které složce |
| **Branch protection** | Pravidlo, které chrání větev `main` (zakáže přímý zápis a vyžaduje schválení) |
| **Release a tag** | Pojmenovaný stav repozitáře, např. `milnik-1`. Označuje odevzdanou verzi |
| **Fabric workspace** | Pracovní prostor v Microsoft Fabric, kde máte lakehouse, notebooky, pipeline apod. |
| **Git integrace** | Propojení Fabric workspace s jednou větví a složkou v repozitáři |
| **Personal Access Token (PAT)** | „Heslo pro aplikace“. Jím Fabric prokazuje, že smí pracovat s repozitářem |
 
---
 
## 3. Jak vytvořit týmový repozitář a dostat přístup
 
Tým si zvolí jednoho **správce**. Správce vytvoří repozitář a nastaví ho, ostatní se připojí.
 
### Krok 1: Vytvoř si účet na GitHubu (všichni)
 
1. Jdi na [github.com](https://github.com) a klikni na **„Sign up“**
2. Vyplň e-mail, heslo a uživatelské jméno
3. Potvrď e-mail
> 💡 **Doporučeno pro správce:** požádej o [GitHub Student Developer Pack](https://education.github.com/) se školním e-mailem. Získáš GitHub Pro zdarma, který je potřeba pro ochranu větve `main` u soukromého repozitáře (kapitola 4). Ověření studentského statusu může trvat několik dní, proto o něj požádej hned.
 
### Krok 2: Vytvoř repozitář (správce)
 
1. Na GitHubu klikni vpravo nahoře na **„+“** → **„New repository“**
2. **Owner:** tvůj účet
3. **Repository name:** `4IT433_Team_XX`, kde XX je dvoumístné číslo týmu (např. `4IT433_Team_01`)
4. Zvol **Private**
5. Zaškrtni **„Add a README file“**. Fabric se umí připojit jen k repozitáři, který už má aspoň jeden commit
6. Klikni **„Create repository“**
> ⚠️ **Repozitář nikdy nenastavuj jako veřejný.** Všechny týmy řeší stejné zadání.
 
### Krok 3: Přidej členy týmu a lektory (správce)
 
1. V repozitáři otevři **„Settings“** → **„Collaborators“** → **„Add people“**
2. Přidej všechny členy týmu podle jejich uživatelských jmen na GitHubu
3. Přidej lektory: **[doplnit uživatelská jména lektorů na GitHubu]**
### Krok 4: Přijmi pozvánku (ostatní členové)
 
1. Zkontroluj e-mail
2. Klikni na odkaz v pozvánce a pak na **„Accept invitation“**
Bez přijaté pozvánky nemůžeš do repozitáře zapisovat ani si k němu vytvořit token.
 
---
 
## 4. Jak nastavit CODEOWNERS, ochranu větve `main` a větev `dev`
 
> 👤 **Tuto kapitolu dělá pouze správce**, hned po vytvoření repozitáře a dřív, než ostatní začnou pracovat.
 
### K čemu je CODEOWNERS?
 
`CODEOWNERS` je textový soubor, ve kterém je napsáno, **kdo „vlastní“ kterou složku nebo soubor**. Když někdo otevře Pull Request, GitHub vlastníky změněných složek **automaticky přiřadí jako reviewery**. Pokud je zapnutá ochrana větve, **nepustí merge**, dokud změny neschválí aspoň jeden z vlastníků.
 
### Krok 1: Zjisti uživatelská jména členů týmu
 
Do CODEOWNERS se píší **uživatelská jména na GitHubu** se zavináčem, např. `@jan-novak`. Ne skutečná jména, ne školní e-maily. Každý vlastník musí mít přijatou pozvánku do repozitáře.
 
### Krok 2: Vytvoř soubor CODEOWNERS
 
1. Otevři repozitář na hlavní stránce (větev `main`)
2. Klikni **„Add file“** → **„Create new file“**
3. Do pole pro název napiš přesně: `.github/CODEOWNERS` (soubor nemá žádnou příponu)
4. Do obsahu vlož pravidla podle vzoru níže a uprav jména
5. Commit message: např. `Přidán soubor CODEOWNERS`
6. Klikni **„Commit changes“**
> ℹ️ Tohle je **jediná výjimka**, kdy se commituje přímo do `main`. Ochrana větve ještě není zapnutá a CODEOWNERS musí být v `main`, jinak nefunguje.
 
#### Vzor obsahu souboru
 
```
# Řádky začínající # jsou komentáře.
# Formát: <cesta>   <vlastník> <vlastník> ...
 
# Výchozí pravidlo pro celý repozitář: schvaluje kdokoli z týmu
*                   @jan-novak @eva-svobodova @petr-dvorak @lucie-kralova @tomas-cerny
 
# Položky synchronizované z Fabric workspace
/fabric/            @jan-novak @eva-svobodova @petr-dvorak @lucie-kralova @tomas-cerny
 
# Nastavení repozitáře: správce a jeden zástupce
/.github/           @jan-novak @eva-svobodova
```
 
> ⚠️ **Platí poslední odpovídající řádek.** Obecné pravidlo `*` dávej vždy nahoru, konkrétní složky pod něj.
 
> ⚠️ **U každé složky uveď aspoň 2 lidi.** Autor Pull Requestu nemůže schválit sám sebe.
 
### Krok 3: Zapni ochranu větve `main`
 
1. V repozitáři otevři **„Settings“** → **„Branches“**
2. Klikni **„Add classic branch protection rule“**
3. **Branch name pattern:** `main` (jen `main`, **ne** `dev`)
4. Zaškrtni:
   - ✅ **„Require a pull request before merging“**
   - ✅ **„Require approvals“**, počet **1**
   - ✅ **„Require review from Code Owners“**
   - ✅ **„Do not allow bypassing the above settings“**
5. Klikni **„Create“**
> ⚠️ **Větev `dev` nechráň.** Fabric do ní commituje přímo a ochrana by commity zablokovala.
 
> ℹ️ Pokud ti GitHub ochranu u soukromého repozitáře nedovolí uložit, potřebuješ GitHub Pro (Student Developer Pack, kapitola 3). Pro stačí jen správci jako vlastníkovi repozitáře. Než ti ho GitHub schválí, platí pravidlo „do `main` jen přes PR“ dohodou. Lektoři v historii repozitáře vidí, jestli se do `main` zapisovalo přímo.
 
### Krok 4: Vytvoř větev `dev` (vždy, i bez ochrany větve)
 
1. Na hlavní stránce repozitáře klikni na tlačítko s názvem větve **`main`** (vlevo nahoře)
2. Do textového pole napiš `dev`
3. Klikni **„Create branch: dev from main“**
### Krok 5: Vypni automatické mazání větví
 
1. **„Settings“** → **„General“** → sekce **„Pull Requests“**
2. Zkontroluj, že **„Automatically delete head branches“** **není** zaškrtnuté
Jinak by GitHub po prvním merge smazal větev `dev` a Fabric by ztratil propojení.
 
---
 
## 5. Jak propojit Fabric workspace s repozitářem
 
### Jak to funguje
 
Celý tým pracuje v **jednom workspace DEV** a ten je propojený s větví **`dev`** a složkou **`fabric`**. Když ve Fabricu klikneš na **Commit**, změny se zapíšou do větve `dev`. Do `main` se dostanou přes Pull Request (kapitola 7).
 
| Workspace | Propojená větev | Složka | K čemu slouží |
|---|---|---|---|
| Týmový DEV (např. `4IT433_01_DEV`) | `dev` | `fabric` | Tady celý tým pracuje a commituje |
 
> ⚠️ **Nepropojuj workspace s větví `main`.** Je chráněná, commit by selhal, a i kdyby prošel, obešel by schvalování.
 
> ℹ️ Vlastní workspace pro každého člena nepoužíváme: studenti workspaces nezakládají a data se přes Git nepřenášejí, takže každý by musel načítat data znovu.
 
### Předpoklady
 
- Workspace běží na **Fabric kapacitě**
- Máš ve workspace roli **Admin**
- V tenantu je povolená synchronizace s GitHubem. Pokud ve Fabricu GitHub vůbec nevidíš, ozvi se vyučujícímu
- Máš přijatou pozvánku do repozitáře (kapitola 3)
### Krok 1: Vytvoř si Personal Access Token (všichni)
 
Fabric se do GitHubu nepřihlašuje heslem, ale tokenem. **Druh tokenu závisí na tom, jestli jsi vlastníkem repozitáře.** GitHub nedovoluje použít fine-grained token pro repozitář, kde jsi jen spolupracovník.
 
#### Správce (vlastník repozitáře): fine-grained token
 
1. Na GitHubu klikni vpravo nahoře na profilový obrázek → **„Settings“**
2. Úplně dole vlevo klikni **„Developer settings“**
3. **„Personal access tokens“** → **„Fine-grained tokens“** → **„Generate new token“**
4. Vyplň:
   - **Token name:** `Fabric 4IT433`
   - **Expiration:** vlastní datum **31. 1. 2027** (výchozí platnost je kratší a připojení by přestalo fungovat uprostřed semestru)
   - **Resource owner:** tvůj účet
   - **Repository access:** **„Only select repositories“** → `4IT433_Team_XX`
   - **Permissions** → **Repository permissions** → **Contents** → **„Read and write“**
5. Klikni **„Generate token“** a token **hned zkopíruj**. GitHub ho ukáže jen jednou
#### Ostatní členové týmu: klasický token
 
1. **„Settings“** → **„Developer settings“** → **„Personal access tokens“** → **„Tokens (classic)“**
2. **„Generate new token“** → **„Generate new token (classic)“**
3. Vyplň:
   - **Note:** `Fabric 4IT433`
   - **Expiration:** vlastní datum **31. 1. 2027**
   - **Scopes:** zaškrtni **`repo`**
4. Klikni **„Generate token“** a token **hned zkopíruj**
> ⚠️ Klasický token dává přístup ke **všem** repozitářům, ke kterým máš přístup. Proto ho nikomu neukazuj a po skončení semestru ho smaž.
 
> 🔒 Token je jako heslo. **Nikdy ho nevkládej do repozitáře, notebooku, chatu ani sdíleného dokumentu.**
 
### Krok 2: Propoj workspace s větví `dev` (pouze správce)
 
1. Otevři týmový workspace DEV ve Fabricu
2. Klikni na **„Workspace settings“** → **„Git integration“** → **„GitHub“**
3. Klikni **„Add account“** a vyplň:
   - **Display name:** např. `GitHub 4IT433`
   - **Personal access token:** tvůj token
   - **Repository URL:** `https://github.com/<vlastnik>/4IT433_Team_XX`
4. Klikni **„Add“** a pak **„Connect“**
5. Vyber:
   - **Repository:** `4IT433_Team_XX`
   - **Branch:** **`dev`**
   - **Folder:** `fabric`
6. Klikni **„Connect and sync“**
Při prvním propojení Fabric porovná workspace a složku v repozitáři:
 
- je-li **složka prázdná**, nabídne uložení obsahu workspace do Gitu,
- je-li **workspace prázdný**, stáhne do něj obsah z Gitu,
- jsou-li **plné obojí**, zeptá se, kterým směrem synchronizovat. Pokud si nejsi jistý, zastav se a zeptej se vyučujícího.
### Krok 3: Přidej svůj účet (ostatní členové)
 
Workspace je už propojený. Přihlášení ke GitHubu je ale ve Fabricu **pro každého uživatele zvlášť**, takže každý, kdo chce commitovat, potřebuje vlastní účet s vlastním tokenem.
 
1. Otevři týmový workspace DEV a klikni na **„Source control“**
2. Fabric tě vyzve k přihlášení ke Git účtu. Přidej účet se **svým** tokenem a URL repozitáře
> ⚠️ **Workspace neodpojuj a neměň větev.** Propojení spravuje jen správce. Odpojení nebo změna větve by se projevila všem členům týmu.
 
### Krok 4: Běžná práce: Commit a Update
 
V horní liště workspace je ikona **„Source control“** s číslem. To je počet změn, které čekají.
 
**Uložení změn z Fabricu do GitHubu (Commit):**
 
1. Klikni na **„Source control“**
2. Na záložce **„Changes“** zaškrtni **jen položky, na kterých jsi pracoval ty**
3. Napiš smysluplný popis změny, např. `bronze: Copy job pro všech 9 tabulek`
4. Klikni **„Commit“**
**Stažení změn z GitHubu do Fabricu (Update):**
 
1. Klikni na **„Source control“**
2. Na záložce **„Updates“** klikni **„Update all“**
U každé položky ve workspace vidíš její stav, např. **Synced** (vše uloženo), **Uncommitted** (změna ještě není v Gitu), **Update required** (v Gitu je novější verze) nebo **Conflict** (změna na obou stranách, požádej o pomoc správce nebo vyučujícího).
 
> 💡 Všech pět členů pracuje ve stejném workspace. Domluvte se, kdo upravuje kterou položku, ať si nepřepisujete práci.
 
---
 
## 6. Jak upravit soubor přímo na GitHubu
 
Na GitHubu upravujete hlavně **README** a další dokumentaci mimo složku `fabric`.
 
> ⚠️ **Na GitHubu pracuj vždy ve větvi `dev`, nikdy v `main`.**
 
> ⚠️ **Soubory ve složce `fabric/` na GitHubu neupravuj.** Ty spravuje Fabric. Pokud je přesto změníš, musí pak někdo ve Fabricu kliknout na **Update**.
 
### Úprava existujícího souboru
 
1. Na hlavní stránce repozitáře přepni větev z `main` na **`dev`** (tlačítko vlevo nahoře)
2. Klikni na soubor a pak na **ikonu tužky** (Edit)
3. Proveď změny
4. Klikni **„Commit changes“**, napiš popis a zkontroluj, že je vybrané **„Commit directly to the `dev` branch“**
5. Potvrď **„Commit changes“**
### Nahrání nového souboru
 
1. Přepni větev na **`dev`**
2. **„Add file“** → **„Upload files“** a přetáhni soubor do okna
3. Zkontroluj, že jsi ve větvi `dev`, napiš popis a klikni **„Commit changes“**
---
 
## 7. Jak vytvořit Pull Request z `dev` do `main`
 
Pull Request otevírejte po dokončení ucelené části (např. Bronze, Silver), a vždy před odevzdáním milníku.
 
### Krok 1: Otevři nový Pull Request
 
1. V repozitáři otevři záložku **„Pull requests“** → **„New pull request“**
2. Nastav **base: `main`** ← **compare: `dev`**
3. Zkontroluj změny a klikni **„Create pull request“**
> ℹ️ Po commitu z Fabricu se může zobrazit žlutý banner **„Compare & pull request“**. Můžeš ho použít, jen zkontroluj, že cílová větev (base) je `main`.
 
### Krok 2: Vyplň Pull Request
 
- **Title:** stručný název, např. `Milník 1: Bronze a Silver`
- **Description:** co se změnilo, jak jste to ověřili (např. počty řádků sedí se zdrojem), co zbývá
Klikni **„Create pull request“**.
 
### Krok 3: Počkej na schválení a slouč
 
- GitHub díky CODEOWNERS automaticky přiřadí reviewery
- Po schválení klikni **„Merge pull request“** → **„Create a merge commit“** → **„Confirm merge“**
> ⚠️ **Po merge nemaž větev `dev`!** GitHub nabízí tlačítko **„Delete branch“**, neklikej na něj. Fabric workspace je s `dev` propojený a pracuje se v ní dál.
 
---
 
## 8. Jak schválit Pull Request
 
Schvaluje vždy **jiný člen týmu než autor** Pull Requestu.
 
1. Otevři záložku **„Pull requests“** a klikni na Pull Request
2. Na záložce **„Files changed“** si prohlédni změny. Notebooky jsou uložené jako čitelné soubory `.py`, nastavení pipeline jako JSON
3. Klikni **„Review changes“** a vyber:
   - ✅ **„Approve“**: změny jsou v pořádku
   - 💬 **„Comment“**: mám komentář, ale neschvaluji ani nezamítám
   - ❌ **„Request changes“**: jsou potřeba opravy
4. Klikni **„Submit review“**
Když reviewer požádá o opravy, autor je udělá ve Fabricu a commitne do `dev`. Pull Request se tím aktualizuje sám, nový otevírat nemusíš.
 
---
 
## 9. Jak označit odevzdaný stav
 
Po sloučení Pull Requestu do `main` správce označí odevzdanou verzi:
 
1. Na hlavní stránce repozitáře klikni vpravo na **„Releases“** → **„Create a new release“**
2. **„Choose a tag“** → napiš název milníku, např. `milnik-1` → **„Create new tag“**
3. **Target:** **`main`**
4. **Release title:** např. `Milník 1`
5. Klikni **„Publish release“**
---
 
## 10. Pravidla repozitáře
 
| Pravidlo | Popis |
|---|---|
| ✅ Fabric workspace DEV je propojený s `dev` | Propojení spravuje jen správce |
| ✅ Na GitHubu upravuj soubory jen ve větvi `dev` | Hlavně README a dokumentaci |
| ✅ Piš smysluplné commit zprávy | Ne jen „update“ nebo „fix“, platí i pro commity z Fabricu |
| ✅ Do `main` jen přes Pull Request | Schvaluje jiný člen týmu než autor |
| ✅ Odevzdaný stav označ releasem | Tag na větvi `main`, např. `milnik-1` |
| ❌ Nezapisuj přímo do `main` | Při zapnuté ochraně to GitHub zablokuje, bez ní to lektoři uvidí v historii |
| ❌ Nemaž větev `dev` | Fabric by ztratil propojení |
| ❌ Neupravuj na GitHubu složku `fabric/` | Spravuje ji Fabric |
| ❌ Neupravuj CODEOWNERS ani `.github` | Pouze správce |
| ❌ Nesdílej svůj token | Ani v repozitáři, notebooku, chatu nebo dokumentu |
 
---
 
## 11. Časté chyby a jak je opravit
 
### ❌ „Merging is blocked“
 
**Příčina:** Pull Request nemá požadované schválení.
**Řešení:** Požádej jiného člena týmu, aby PR schválil.
 
### ❌ „Review required: Code owner review required“
 
**Příčina:** PR schválil někdo, kdo není v CODEOWNERS vlastníkem změněné složky, nebo jsi jediným vlastníkem ty sám.
**Řešení:** Podívej se do `.github/CODEOWNERS`, kdo složku vlastní, a požádej o schválení jeho. Pokud je u složky jen jedno jméno, požádej správce, aby doplnil další.
 
### ❌ Při přidání účtu se nezobrazí repozitář nebo „Can't fetch branches“
 
**Příčina:** Token k repozitáři nemá přístup. Typicky jsi vytvořil fine-grained token, ale nejsi vlastníkem repozitáře, nebo jsi ještě nepřijal pozvánku.
**Řešení:** Přijmi pozvánku (kapitola 3) a vytvoř klasický token s oprávněním `repo` (kapitola 5, Krok 1).
 
### ❌ Commit z Fabricu selže
 
**Příčina:** Workspace je propojený s chráněnou větví `main`, token nemá oprávnění pro zápis, nebo vypršela jeho platnost.
**Řešení:** Ověř u správce, že je workspace propojený s `dev`. Pokud ano, vytvoř nový token s oprávněním pro zápis a aktualizuj svůj účet v Git integraci.
 
### ❌ Někdo po merge smazal větev `dev`
 
**Řešení:** Otevři sloučený Pull Request a klikni na **„Restore branch“**. Větev se obnoví se stejným názvem.
 
### ❌ Commitoval jsem přímo do `main`
 
**Příčina:** Ochrana větve není zapnutá a soubor jsi upravil na GitHubu bez přepnutí na `dev`.
**Řešení:** Nic nemaž a dej vědět správci. Další změny dělej ve větvi `dev`.
 
### ❌ Položka ve workspace je ve stavu „Conflict“
 
**Příčina:** Stejná položka se změnila ve workspace i v repozitáři.
**Řešení:** Nic neřeš sám, ozvi se správci nebo vyučujícímu.
 
### ❌ Ve Fabricu nevidím možnost GitHub nebo Git integration
 
**Příčina:** Workspace neběží na Fabric kapacitě, nemáš ve workspace roli Admin, nebo není v tenantu povolená synchronizace s GitHubem.
**Řešení:** Zkontroluj svou roli ve workspace. Pokud je v pořádku, kontaktuj vyučujícího.
 
### ❌ Synchronizace s GitHubem najednou přestala fungovat
 
**Příčina:** Vypršela platnost tokenu.
**Řešení:** Vygeneruj nový token a aktualizuj svůj účet v Git integraci.
 
### ❌ Na GitHubu nevidím data z lakehouse
 
**Příčina:** Tak to má být. Do Gitu se ukládají jen definice položek, ne data (kapitola 1).
