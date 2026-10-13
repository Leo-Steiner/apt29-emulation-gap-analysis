# Angriffsnarrative – APT29 Emulation

## Szenario

Die Emulation bildet einen Angriff auf eine fiktive österreichische Organisation im öffentlichen Sektor nach. Die Organisation ist als anonymisiertes Szenario dargestellt und dient ausschließlich als Testumgebung für die Bewertung von Detektionsfähigkeiten.

Der Angriff beginnt mit einer Phishing-E-Mail, die an einen Mitarbeiter der Organisation gesendet wird. Die E-Mail enthält einen Link, der auf eine kompromittierte Webseite führt, von der eine schädliche Datei heruntergeladen wird. Der Mitarbeiter führt die Datei aus (User Execution: Malicious File, T1204.002), wodurch der Angreifer ersten Zugriff auf den Arbeitsplatz erhält. Anschließend wird PowerShell genutzt, um eine Command-and-Control-Verbindung zum Angreifer-Server aufzubauen (Command and Scripting Interpreter: PowerShell, T1059.001; Application Layer Protocol: Web Protocols, T1071.001).

Zur Sicherung des Zugriffs legt der Angreifer einen Registry Run Key an (Registry Run Keys / Startup Folder, T1547.001) und erstellt eine geplante Aufgabe (Scheduled Task, T1053.005). Danach versucht der Angreifer, Sicherheitstools zu deaktivieren (Impair Defenses: Disable or Modify Tools, T1562.001) und führt Process Injection durch (Process Injection, T1055), um sich vor Erkennung zu schützen.

Im nächsten Schritt werden Credentials aus dem LSASS-Speicher extrahiert (OS Credential Dumping: LSASS Memory, T1003.001) und ungesicherte Credentials ausgelesen (Unsecured Credentials, T1552). Der Angreifer führt System-, Account- und Netzwerk-Discovery durch (System Information Discovery, T1082; Account Discovery, T1087; System Network Configuration Discovery, T1016), um sich im Netzwerk zu orientieren.

Anschließend bewegt sich der Angreifer lateral über Remote Services (T1021) auf weitere Systeme. Die gesammelten Daten werden archiviert (Archive Collected Data, T1560) und über den C2-Kanal exfiltriert (Exfiltration Over C2 Channel, T1041).
