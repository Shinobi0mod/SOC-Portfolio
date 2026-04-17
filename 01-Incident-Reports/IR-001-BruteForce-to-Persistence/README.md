[Escalation] IR-001: Brute Force & Persistence Detected on DESKTOP-GHN4O5G
Status: Escalated (True Positive)
Severity: High
Assigned Analyst: [Sviatoslav]
1. Описание инцидента (Summary)
В ходе проактивного мониторинга (Threat Hunting) выявлена цепочка событий, указывающая на успешную компрометацию учетной записи "1". Зафиксирован переход от подбора пароля (Brute Force) к закреплению в системе (Persistence) через модификацию реестра.
2. Детали анализа (Evidence)
Initial Access: 6 событий Event ID 4625 (Failure) с одного IP (127.0.0.1) в течение 1 минуты, завершившихся успешным входом Event ID 4624 (Success) в 14:57:27.
Execution: Запуск powershell.exe с аргументами -ExecutionPolicy Bypass -File C:\Temp\svchost.exe. Использование системного имени процесса в директории Temp расценивается как попытка маскировки (Masquerading).
File Activity: Event ID 11 (Sysmon) подтверждает создание файла C:\Temp\svchost.exe процессом powershell.exe за 11 секунд до запуска.
Persistence: Обнаружена модификация ключа реестра HKCU\Software\Microsoft\Windows\CurrentVersion\Run\TestApp. Значение указывает на подозрительный файл в C:\Temp.
3. Вердикт и Эскалация (Conclusion)
Активность классифицирована как True Positive. Признаков легитимной деятельности администратора не обнаружено.
Рекомендация для T2:
Изоляция хоста DESKTOP-GHN4O5G.
Принудительная смена пароля пользователя "1".
Очистка артефактов (файл в Temp и ключ в Run).

Скрини офрмлени ниже 
![Brute Force Screen](./screenshot$/bruteForce.png)
![Logon Screen](./screenshot$/seccessfulLogin.png)
![Process Screen](./screenshot$/process.png)
![File Creation Screen](./screenshot$/file.png)
![Registry Screen](./screenshot$/Reg.png)
