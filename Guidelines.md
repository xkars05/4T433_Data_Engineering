# 📘 Návod pro práci s repozitářem

Tento dokument je určen všem členům týmů. Přečti si ho celý před tím, než začneš pracovat.

---

## 📋 Obsah

1. [Co je GitHub a jak funguje?](#1-co-je-github-a-jak-funguje)
2. [Základní pojmy](#2-základní-pojmy)
3. [Jak se přihlásit a dostat přístup](#3-jak-se-přihlásit-a-dostat-přístup)
4. [Jak nastavit CODEOWNERS a ochranu větve main](#4-jak-nastavit-codeowners-a-ochranu-větve-main)
5. [Jak propojit Fabric workspace s repozitářem](#5-jak-propojit-fabric-workspace-s-repozitářem)
6. [Jak nahrát nebo upravit soubor](#6-jak-nahrát-nebo-upravit-soubor)
7. [Jak vytvořit Pull Request](#7-jak-vytvořit-pull-request)
8. [Jak schválit Pull Request](#8-jak-schválit-pull-request)
9. [Pravidla repozitáře](#9-pravidla-repozitáře)
10. [Časté chyby a jak je opravit](#10-časté-chyby-a-jak-je-opravit)

---

## 1. Co je GitHub a jak funguje?

GitHub je platforma pro sdílení a správu souborů. Funguje jako sdílená složka, ale s jedním velkým rozdílem — **každá změna je zaznamenána, kdo ji udělal a kdy**. Nikdy nemůžeš omylem smazat práci někoho jiného, protože vše je verzované.

---

## 2. Základní pojmy

| Pojem | Vysvětlení |
|---|---|
| **Repozitář (repo)** | Složka se všemi soubory projektu |
| **Branch (větev)** | Kopie repozitáře, kde pracuješ izolovaně |
| **Commit** | Uložení změn s popisem co jsi změnil |
| **Pull Request (PR)** | Žádost o přidání tvých změn do hlavní větve |
| **Merge** | Sloučení tvých změn do hlavní větve `main` |
| **main** | Hlavní, oficiální větev — vždy stabilní |
| **CODEOWNERS** | Soubor, který určuje kdo schvaluje změny v jaké složce |
| **Branch protection** | Pravidlo, které chrání větev `main` (např. zakáže přímý push a vyžaduje schválení) |
| **Fabric workspace** | Pracovní prostor v Microsoft Fabric, kde máte lakehouse, notebooky, reporty apod. |
| **Git integrace** | Propojení Fabric workspace s větví v GitHubu — položky z Fabricu se ukládají do repozitáře jako soubory |
| **Personal Access Token (PAT)** | „Heslo pro aplikace" — jím Fabric prokazuje, že smí pracovat s tvým repozitářem |

---

## 3. Jak se přihlásit a dostat přístup

### Krok 1 — Vytvoř si GitHub účet
1. Jdi na [github.com](https://github.com)
2. Klikni na **"Sign up"**
3. Vyplň email, heslo a uživatelské jméno
4. Potvrď email

### Krok 2 — Vytvořte svoje týmové GitHub repository
- Jeden ze členů týmu jako správce vytvoří nové repository
- Repository pojmenuj 4IT433_Team_XX
- Správce přidá všechny členy týmu
- Dostaneš emailovou pozvánku — **musíš ji přijmout!**

### Krok 3 — Přijmi pozvánku
1. Zkontroluj email
2. Klikni na odkaz v pozvánce
3. Klikni **"Accept invitation"**

---

## 4. Jak nastavit CODEOWNERS a ochranu větve `main`

> 👤 **Tuto sekci dělá pouze správce repozitáře**, a to hned po vytvoření repozitáře — ještě předtím, než ostatní začnou pracovat.

### K čemu je CODEOWNERS?

`CODEOWNERS` je obyčejný textový soubor, ve kterém je napsáno, **kdo „vlastní" kterou složku nebo soubor**. Když někdo otevře Pull Request, který mění soubory v dané složce, GitHub:

- **automaticky přiřadí** vlastníky té složky jako reviewery (nemusíš je hledat ručně),
- a pokud je zapnutá ochrana větve, **nepustí merge**, dokud změny neschválí aspoň jeden z vlastníků.

Díky tomu se nestane, že by někdo změnil soubor, aniž by se na to podíval člověk, který za něj zodpovídá.

> ⚠️ Samotný soubor CODEOWNERS reviewery jen **přiřazuje**. Aby bylo schválení **povinné**, musí se zapnout ještě ochrana větve `main` (viz Krok 3).

### Krok 1 — Zjisti uživatelská jména členů týmu
Do CODEOWNERS se píší **GitHub uživatelská jména** se zavináčem, např. `@jan-novak`. Ne skutečná jména, ne školní emaily. Každý člen týmu ti své jméno najde vpravo nahoře po kliknutí na svůj profilový obrázek.

> ℹ️ Vlastník musí mít do repozitáře přístup pro zápis (**Write**) — musí tedy už mít přijatou pozvánku z kapitoly 3.

### Krok 2 — Vytvoř soubor CODEOWNERS
1. Otevři repozitář na hlavní stránce (větev `main`)
2. Klikni na **"Add file"** → **"Create new file"**
3. Do pole pro název napiš přesně: `.github/CODEOWNERS`
   - Jakmile napíšeš lomítko `/`, GitHub automaticky vytvoří složku `.github`
   - Soubor **nemá žádnou příponu** (ne `.txt`, ne `.md`)
4. Do obsahu vlož pravidla podle vzoru níže a uprav jména
5. Do pole **"Commit message"** napiš např. `Přidán soubor CODEOWNERS`
6. Klikni **"Commit changes"**

> ℹ️ Tohle je **jediná výjimka**, kdy se commituje přímo do `main` — ochrana větve ještě není zapnutá. CODEOWNERS musí být v `main`, jinak nefunguje.

### Vzor obsahu souboru

```
# Řádky začínající # jsou komentáře.
# Formát: <cesta>   <vlastník> <vlastník> ...

# Výchozí pravidlo pro celý repozitář — schvaluje kdokoli z týmu
*                   @jan-novak @eva-svobodova @petr-dvorak @lucie-kralova

# Položky synchronizované z Fabric workspace
/fabric/            @jan-novak @lucie-kralova

# Nastavení repozitáře — pouze správce
/.github/           @jan-novak
```

### Jak pravidla číst

> ⚠️ **Platí poslední odpovídající řádek.** Když soubor odpovídá více pravidlům, použije se to, které je v souboru **níž**. Proto dávej obecné pravidlo `*` vždy **nahoru** a konkrétní složky pod něj.

> ⚠️ **U každé složky uveď alespoň 2 lidi.** Autor Pull Requestu nemůže schválit sám sebe. Kdyby byl u složky jediný vlastník a otevřel PR právě on, nikdo jiný by ho nemohl schválit.

### Krok 3 — Zapni ochranu větve `main`
1. V repozitáři klikni na **"Settings"** (ozubené kolo v horním menu)
2. V levém menu vyber **"Branches"**
3. Klikni na **"Add classic branch protection rule"**
4. Do **"Branch name pattern"** napiš `main`
5. Zaškrtni:
   - ✅ **"Require a pull request before merging"**
   - ✅ **"Require approvals"** → počet nastav na **1**
   - ✅ **"Require review from Code Owners"**
   - ✅ **"Do not allow bypassing the above settings"** (pravidla pak platí i pro správce)
6. Dole klikni **"Create"**

> ℹ️ Pokud je repozitář **privátní** a GitHub ti ochranu větve nedovolí uložit, potřebuješ účet GitHub Pro. Studenti ho mají zdarma přes [GitHub Education](https://education.github.com/) (Student Developer Pack). Druhou možností je nechat repozitář veřejný.

### Krok 4 — Ověř, že to funguje
1. Otevři soubor `.github/CODEOWNERS` v repozitáři
2. Pokud je v souboru chyba (např. neexistující uživatel), GitHub ji **podtrhne a ukáže varování** přímo u řádku
3. Nech kolegu udělat zkušební PR — měl by se mu automaticky přiřadit reviewer

---

## 5. Jak propojit Fabric workspace s repozitářem

### K čemu to je?

Microsoft Fabric umí ukládat obsah workspace (notebooky, lakehouse, semantic modely, reporty, pipeline…) **jako soubory do GitHubu**. Díky tomu:

- máte **historii všech změn** i ve Fabricu — kdo co změnil a kdy,
- můžete se vrátit ke starší verzi,
- změny ve Fabricu procházejí **schvalováním přes Pull Request**.

### Jak to funguje v praxi

Fabric workspace je vždy propojený s **jednou konkrétní větví** a **jednou složkou** v repozitáři. Když ve Fabricu klikneš na **Commit**, změny se zapíšou **přímo do té větve**.

> ⚠️ **Nepropojuj svůj pracovní workspace s větví `main`!** Větev `main` je chráněná (kapitola 4), takže by commit z Fabricu selhal. A i kdyby prošel, obešel bys schvalování.

Doporučené uspořádání:

| Workspace | Propojená větev | K čemu slouží |
|---|---|---|
| `4IT433_Team_XX` (týmový) | `main` | Jen **čtení** — po schválení PR si do něj stáhnete aktuální stav (**Update**). Nic se z něj necommituje. |
| `4IT433_Team_XX_jmeno` (tvůj) | `feature/...` | Tady **pracuješ**, commituješ a z této větve pak otevíráš Pull Request |

### Předpoklady
- Workspace běží na **Fabric kapacitě** (stačí i Trial)
- Ve workspace máš roli **Admin** (jen admin může workspace propojit nebo odpojit)
- Správce Fabric tenantu (vyučující) má zapnuté nastavení **"Users can synchronize workspace items with GitHub repositories"** — pokud ve Fabricu GitHub vůbec nevidíš, ozvi se vyučujícímu
- Máš přijatou pozvánku do týmového repozitáře (kapitola 3)

### Krok 1 — Vytvoř si Personal Access Token v GitHubu
Fabric se do GitHubu nepřihlašuje heslem, ale tokenem.

1. Na GitHubu klikni vpravo nahoře na svůj profilový obrázek → **"Settings"**
2. Úplně dole v levém menu klikni na **"Developer settings"**
3. Vyber **"Personal access tokens"** → **"Fine-grained tokens"**
4. Klikni **"Generate new token"**
5. Vyplň:
   - **Token name:** např. `Fabric 4IT433`
   - **Expiration:** do konce semestru
   - **Resource owner:** vlastník repozitáře
   - **Repository access:** **"Only select repositories"** → vyber `4IT433_Team_XX`
   - **Permissions** → **Repository permissions** → **Contents** → **"Read and write"**
6. Klikni **"Generate token"**
7. Token **hned zkopíruj** — GitHub ti ho ukáže jen jednou!

> 🔒 Token je jako heslo. **Nikdy ho nevkládej do repozitáře, do chatu ani do sdíleného dokumentu.**

> ℹ️ Pokud nejsi vlastníkem repozitáře a v nabídce **Repository access** ho nevidíš, vytvoř místo toho klasický token: **"Tokens (classic)"** → **"Generate new token (classic)"** → zaškrtni oprávnění **`repo`**. Pozor, klasický token dává přístup ke **všem** tvým repozitářům, proto mu nastav krátkou platnost.

### Krok 2 — Přidej GitHub účet do Fabricu
1. Otevři svůj workspace ve Fabricu
2. Klikni na **"Workspace settings"** (nastavení pracovního prostoru)
3. V levém menu vyber **"Git integration"**
4. Vyber **"GitHub"**
5. Klikni **"Add account"** a vyplň:
   - **Display name:** libovolný název, např. `Můj GitHub`
   - **Personal access token:** vlož zkopírovaný token
   - **Repository URL:** `https://github.com/<vlastnik>/4IT433_Team_XX` — vyplň ho, ať se omylem nepřipojíš k jinému repozitáři
6. Klikni **"Add"**

> ℹ️ Přihlášení ke GitHubu je ve Fabricu **pro každého uživatele zvlášť**. I když je workspace už propojený, každý, kdo z něj chce commitovat, si musí přidat svůj vlastní účet s vlastním tokenem.

### Krok 3 — Propoj workspace s větví a složkou
1. Klikni **"Connect"**
2. Vyber:
   - **Repository:** `4IT433_Team_XX`
   - **Branch:** tvoje větev, např. `feature/jan-novak` (ne `main`!)
   - **Folder:** `fabric` — složka v repozitáři, kam se budou ukládat položky z workspace
3. Klikni **"Connect and sync"**

Při prvním propojení Fabric porovná workspace a složku v repozitáři:
- je-li **složka prázdná**, nabídne uložení obsahu workspace do Gitu,
- je-li **prázdný workspace**, stáhne do něj obsah z Gitu,
- jsou-li **plné obojí**, zeptá se, kterým směrem synchronizovat — pokud si nejsi jistý, zastav se a zeptej se správce.

### Krok 4 — Běžná práce: Commit a Update
V horní liště workspace se objeví ikona **"Source control"** s číslem — to je počet změn, které čekají.

**Uložení změn z Fabricu do GitHubu (Commit):**
1. Klikni na **"Source control"**
2. Na záložce **"Changes"** zaškrtni položky, které chceš uložit
3. Napiš smysluplný popis změny (ne jen „update"!)
4. Klikni **"Commit"**

**Stažení změn z GitHubu do Fabricu (Update):**
1. Klikni na **"Source control"**
2. Na záložce **"Updates"** klikni **"Update all"**

U každé položky ve workspace vidíš její stav, např. **Synced** (vše uloženo), **Uncommitted** (změna ještě není v Gitu), **Update required** (v Gitu je novější verze) nebo **Conflict** (změna na obou stranách — požádej správce o pomoc).

### Krok 5 — Pull Request a aktualizace týmového workspace
1. Po commitu z Fabricu jdi na GitHub — uvidíš žlutý banner u své větve
2. Vytvoř Pull Request do `main` přesně podle kapitoly 7
3. GitHub díky CODEOWNERS přiřadí reviewera pro složku `fabric/`
4. Po schválení a merge otevře správce **týmový workspace** (propojený s `main`) a klikne **"Source control"** → **"Updates"** → **"Update all"**

> 💡 **Tip:** Vlastní workspace s vlastní větví si můžeš vytvořit i přímo ve Fabricu: v **"Source control"** → **"Branches"** → **"Branch out to new workspace"**. Fabric založí novou větev i nový workspace a rovnou je propojí.

---

## 6. Jak nahrát nebo upravit soubor

> ⚠️ **NIKDY nepracuj přímo v `main` větvi!** Vždy si vytvoř vlastní branch.

### Krok 1 — Otevři repozitář

### Krok 2 — Vytvoř novou branch
1. Klikni na tlačítko **`main`** vlevo nahoře (šedé tlačítko s ikonou větve)
2. Do textového pole napiš název své větve.
3. Klikni na **"Create branch: feature/..."**

### Krok 3 — Přejdi do složky svého týmu
Klikni na složku svého týmu

### Krok 4a — Nahraj nový soubor
Pokud nahráváš nový soubor:
1. Klikni na **"Add file"** → **"Upload files"**
2. Přetáhni soubor do okna
3. Zkontroluj, že jsi ve správné branch (ne v `main`!)
4. Do pole **"Commit changes"** napiš popis
5. Klikni **"Commit changes"**

### Krok 4b — Uprav existující soubor
Pokud upravuješ existující soubor:
1. Klikni na soubor
2. Klikni na **ikonu tužky** (Edit) vpravo nahoře
3. Proveď změny
4. Dole vyplň popis změny
5. Zkontroluj, že je vybrána možnost Commit directly to tvoje branch, ne main!
6. Klikni **"Commit changes"**

---

## 7. Jak vytvořit Pull Request

Po commitu je potřeba požádat o začlenění změn do `main`. To se dělá přes **Pull Request**.

### Krok 1
Po commitu GitHub automaticky zobrazí žlutý banner:
> *"Your branch had recent pushes — Compare & pull request"*

Klikni na zelené tlačítko **"Compare & pull request"**

### Krok 2 — Vyplň Pull Request
- **Title:** Stručný název
- **Description:** Popiš co jsi přidal/změnil

### Krok 3
Klikni na zelené tlačítko **"Create pull request"**

### Krok 4 — Počkej na schválení
- GitHub automaticky přiřadí reviewera z tvého týmu (díky CODEOWNERS)
- Reviewer dostane emailové upozornění
- Po schválení klikni **"Merge pull request"** → **"Confirm merge"**

---

## 8. Jak schválit Pull Request

Pokud jsi byl označen jako reviewer:

1. Jdi do záložky **"Pull requests"** v repozitáři
2. Klikni na Pull Request který máš schválit
3. Klikni na záložku **"Files changed"** — prohlédni si co bylo změněno
4. Klikni na zelené tlačítko **"Review changes"**
5. Vyber jednu z možností:
   - ✅ **"Approve"** — změny jsou v pořádku, schvaluji
   - 💬 **"Comment"** — mám komentář, ale neschvaluji ani nezamítám
   - ❌ **"Request changes"** — jsou potřeba opravy
6. Klikni **"Submit review"**

---

## 9. Pravidla repozitáře

| Pravidlo | Popis |
|---|---|
| ✅ Pracuj vždy ve vlastní branch | Nikdy ne přímo v `main` |
| ✅ Piš smysluplné commit zprávy | Ne jen "update" nebo "fix" — platí i pro commity z Fabricu |
| ✅ Každý PR musí být schválen | Minimálně 1 schválení od vlastníka složky (CODEOWNERS) je povinné |
| ✅ Svůj Fabric workspace propoj s vlastní branch | Týmový workspace propojený s `main` slouží jen pro Update |
| ❌ Nikdy nepush přímo do `main` | Bude zablokováno systémem |
| ❌ Neupravuj CODEOWNERS ani .github | Pouze správce repozitáře |
| ❌ Nesdílej svůj Personal Access Token | Nevkládej ho do repozitáře, chatu ani dokumentů |

---

## 10. Časté chyby a jak je opravit

### ❌ "Merging is blocked"
**Příčina:** Pull Request nemá požadované schválení.
**Řešení:** Požádej kolegu z tvého týmu aby PR schválil.

### ❌ Commitoval jsem přímo do `main`
**Příčina:** Nezvolil jsi vlastní branch před commitem.
**Řešení:** Vytvoř novou branch, přenes změny a otevři PR. Požádej správce o pomoc.

### ❌ "Review required — Code owner review required"
**Příčina:** PR schválil někdo, kdo není v CODEOWNERS uvedený jako vlastník změněné složky, nebo jsi jediným vlastníkem složky ty sám.
**Řešení:** Podívej se do `.github/CODEOWNERS`, kdo složku vlastní, a požádej o schválení jeho. Pokud je u složky jen jedno jméno, požádej správce, aby doplnil další.

### ❌ CODEOWNERS nikoho automaticky nepřiřazuje
**Příčina:** Soubor není ve větvi `main`, má špatný název nebo umístění (musí být přesně `.github/CODEOWNERS`, bez přípony), uživatelské jméno je napsané špatně, nebo daný člověk ještě nepřijal pozvánku do repozitáře.
**Řešení:** Otevři soubor CODEOWNERS na GitHubu — chybné řádky jsou podtržené s varováním. Oprav je přes PR.

### ❌ Commit z Fabricu selže
**Příčina:** Workspace je propojený s chráněnou větví `main`, nebo token nemá oprávnění **Contents: Read and write**.
**Řešení:** Propoj svůj workspace s vlastní větví (kapitola 5). Pokud jsi propojený správně, vytvoř nový token s oprávněním pro zápis a aktualizuj ho ve **Workspace settings** → **Git integration**.

### ❌ Ve Fabricu nevidím možnost GitHub nebo Git integration
**Příčina:** Workspace neběží na Fabric kapacitě, nemáš ve workspace roli Admin, nebo není v tenantu povolena synchronizace s GitHubem.
**Řešení:** Zkontroluj kapacitu a svou roli ve workspace. Pokud je obojí v pořádku, kontaktuj vyučujícího.

### ❌ Při propojení se nezobrazí můj repozitář
**Příčina:** Token nemá k repozitáři přístup — typicky fine-grained token, když nejsi vlastníkem repozitáře.
**Řešení:** Vytvoř klasický token s oprávněním `repo` (kapitola 5, Krok 1) a přidej účet do Fabricu znovu.

### ❌ Synchronizace s GitHubem najednou přestala fungovat
**Příčina:** Vypršela platnost tokenu.
**Řešení:** Vygeneruj nový token a vlož ho do svého účtu ve **Workspace settings** → **Git integration**.
