# Qdrant Multi-Tenancy Ansible Playbook

Dieses Repository enthält ein Ansible-Playbook mit Rollen zur automatisierten Multi-Tenancy-Konfiguration für Qdrant-Server.

## Projektstruktur

```
.
├── playbook.yml                      # Haupt-Playbook
├── data/
│   ├── students.csv                  # Eingabe: Studierendendaten
│   └── students_with_tokens.csv      # Ausgabe: Tokens pro Studierendem
├── inventory/
│   └── hosts.ini                     # Ansible-Inventar (Platzhalter für echte Daten)
├── roles/
│   └── qdrant_multi_tenancy/
│       ├── defaults/
│       │   └── main.yml
│       ├── tasks/
│       │   ├── create_student_collections.yml
│       │   ├── create_student_empty_collections.yml
│       │   ├── create_student_keys.yml
│       │   ├── create_test_collection.yml
│       │   ├── delete_students_collections.yml
│       │   ├── load_test_data.yml
│       │   ├── main.yml
│       │   ├── wait_for_api.yml
│       │   └── write_stud_csv_with_token.yml
│       ├── templates/
│       └── vars/
├── templates/
│   └── students_with_tokens.csv.j2   # Jinja2-Template für Token-CSV
├── requirements.txt                  # Python-Abhängigkeiten
├── .vault_pass_template.txt          # Vorlage für das Vault-Passwort
└── README.md
```

## Voraussetzungen

- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- Python 3 auf dem Steuerungsrechner
- Zugangsdaten und API-Keys für die Ziel-Qdrant-Server (nicht im Repo!)
- Python-Pakete: `pyjwt`, `requests` (siehe `requirements.txt`)

## Nutzung

1. **Vault-Passwort einrichten:**  
   - Benenne die Datei `.vault_pass_template.txt` in `.vault_pass.txt` um:
     ```bash
     mv .vault_pass_template.txt .vault_pass.txt
     ```
   - Öffne die Datei `.vault_pass.txt` und füge dein Vault-Passwort ein.

2. **Python-Abhängigkeiten installieren:**  
   Installiere die benötigten Python-Pakete mit:
   ```bash
   pip install -r requirements.txt
   ```

3. **Inventar anpassen:**  
   Passe [`inventory/hosts.ini`](inventory/hosts.ini) mit Zielsystemen an (keine echten Zugangsdaten im Repo!).

4. **Studierendendaten einfügen:**  
   Trage Studierende in [`data/students.csv`](data/students.csv) ein.

5. **Playbook ausführen:**  
   Führe das Playbook aus:
   ```bash
   ansible-playbook -i inventory/hosts.ini playbook.yml --vault-password-file .vault_pass.txt
   ```

6. **Ergebnisse prüfen:**  
   Die generierten Tokens findest du danach in `data/students_with_tokens.csv`.

## Rollenbeschreibung

Die Rolle [`qdrant_multi_tenancy`](roles/qdrant_multi_tenancy/) übernimmt:

- Einlesen der Studierenden aus der CSV
- Anlegen von Qdrant-Collections pro Studierendem
- Generieren von individuellen API-Keys (JWTs)
- Schreiben der Token-CSV (`data/students_with_tokens.csv`)
- (Optional) Löschen von Collections
- (Optional) Laden von Testdaten

## Hinweise

- **UTF-8:** Stelle sicher, dass alle Dateien als UTF-8 gespeichert sind.
- **Sensible Daten:** Zugangsdaten und API-Keys gehören nicht ins öffentliche Repository. Nutze Ansible Vault für geheime Variablen.
- **Platzhalter:** Die Datei `hosts.ini` enthält keine echten Zugangsdaten.
- **Erweiterbarkeit:** Weitere Rollen, Tasks oder Templates können ergänzt werden.

## Lizenz

MIT-Lizenz

---