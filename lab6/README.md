# Информационно-аналитические технологии поиска угроз информационной
безопасности
Kabanova Svetlana

# Лабораторная работа №6

Исследование вредоносной активности в домене Windows

## Цель работы

1.  Закрепить навыки исследования данных журнала Windows Active
    Directory
2.  Изучить структуру журнала системы Windows Active Directory
3.  Зекрепить практические навыки использования языка программирования R
    для обработки данных
4.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R

## Ход работы

#### Подготовка данных

1\. Импортируйте данные в R. Это можно выполнить с помощью
jsonlite::stream_in(file()) . Датасет находится по адресу
https://storage.yandexcloud.net/iamcth-data/dataset.tar.gz.

``` r
library(jsonlite)
```

``` r
json_data <- stream_in(file("/home/irina/threat_hunting_2/lab6/dataset/caldera_attack_evals_round1_day1_2019-10-20201108.json"))
```

    opening file input connection.


     Found 500 records...
     Found 1000 records...
     Found 1500 records...
     Found 2000 records...
     Found 2500 records...
     Found 3000 records...
     Found 3500 records...
     Found 4000 records...
     Found 4500 records...
     Found 5000 records...
     Found 5500 records...
     Found 6000 records...
     Found 6500 records...
     Found 7000 records...
     Found 7500 records...
     Found 8000 records...
     Found 8500 records...
     Found 9000 records...
     Found 9500 records...
     Found 10000 records...
     Found 10500 records...
     Found 11000 records...
     Found 11500 records...
     Found 12000 records...
     Found 12500 records...
     Found 13000 records...
     Found 13500 records...
     Found 14000 records...
     Found 14500 records...
     Found 15000 records...
     Found 15500 records...
     Found 16000 records...
     Found 16500 records...
     Found 17000 records...
     Found 17500 records...
     Found 18000 records...
     Found 18500 records...
     Found 19000 records...
     Found 19500 records...
     Found 20000 records...
     Found 20500 records...
     Found 21000 records...
     Found 21500 records...
     Found 22000 records...
     Found 22500 records...
     Found 23000 records...
     Found 23500 records...
     Found 24000 records...
     Found 24500 records...
     Found 25000 records...
     Found 25500 records...
     Found 26000 records...
     Found 26500 records...
     Found 27000 records...
     Found 27500 records...
     Found 28000 records...
     Found 28500 records...
     Found 29000 records...
     Found 29500 records...
     Found 30000 records...
     Found 30500 records...
     Found 31000 records...
     Found 31500 records...
     Found 32000 records...
     Found 32500 records...
     Found 33000 records...
     Found 33500 records...
     Found 34000 records...
     Found 34500 records...
     Found 35000 records...
     Found 35500 records...
     Found 36000 records...
     Found 36500 records...
     Found 37000 records...
     Found 37500 records...
     Found 38000 records...
     Found 38500 records...
     Found 39000 records...
     Found 39500 records...
     Found 40000 records...
     Found 40500 records...
     Found 41000 records...
     Found 41500 records...
     Found 42000 records...
     Found 42500 records...
     Found 43000 records...
     Found 43500 records...
     Found 44000 records...
     Found 44500 records...
     Found 45000 records...
     Found 45500 records...
     Found 46000 records...
     Found 46500 records...
     Found 47000 records...
     Found 47500 records...
     Found 48000 records...
     Found 48500 records...
     Found 49000 records...
     Found 49500 records...
     Found 50000 records...
     Found 50500 records...
     Found 51000 records...
     Found 51500 records...
     Found 52000 records...
     Found 52500 records...
     Found 53000 records...
     Found 53500 records...
     Found 54000 records...
     Found 54500 records...
     Found 55000 records...
     Found 55500 records...
     Found 56000 records...
     Found 56500 records...
     Found 57000 records...
     Found 57500 records...
     Found 58000 records...
     Found 58500 records...
     Found 59000 records...
     Found 59500 records...
     Found 60000 records...
     Found 60500 records...
     Found 61000 records...
     Found 61500 records...
     Found 62000 records...
     Found 62500 records...
     Found 63000 records...
     Found 63500 records...
     Found 64000 records...
     Found 64500 records...
     Found 65000 records...
     Found 65500 records...
     Found 66000 records...
     Found 66500 records...
     Found 67000 records...
     Found 67500 records...
     Found 68000 records...
     Found 68500 records...
     Found 69000 records...
     Found 69500 records...
     Found 70000 records...
     Found 70500 records...
     Found 71000 records...
     Found 71500 records...
     Found 72000 records...
     Found 72500 records...
     Found 73000 records...
     Found 73500 records...
     Found 74000 records...
     Found 74500 records...
     Found 75000 records...
     Found 75500 records...
     Found 76000 records...
     Found 76500 records...
     Found 77000 records...
     Found 77500 records...
     Found 78000 records...
     Found 78500 records...
     Found 79000 records...
     Found 79500 records...
     Found 80000 records...
     Found 80500 records...
     Found 81000 records...
     Found 81500 records...
     Found 82000 records...
     Found 82500 records...
     Found 83000 records...
     Found 83500 records...
     Found 84000 records...
     Found 84500 records...
     Found 85000 records...
     Found 85500 records...
     Found 86000 records...
     Found 86500 records...
     Found 87000 records...
     Found 87500 records...
     Found 88000 records...
     Found 88500 records...
     Found 89000 records...
     Found 89500 records...
     Found 90000 records...
     Found 90500 records...
     Found 91000 records...
     Found 91500 records...
     Found 92000 records...
     Found 92500 records...
     Found 93000 records...
     Found 93500 records...
     Found 94000 records...
     Found 94500 records...
     Found 95000 records...
     Found 95500 records...
     Found 96000 records...
     Found 96500 records...
     Found 97000 records...
     Found 97500 records...
     Found 98000 records...
     Found 98500 records...
     Found 99000 records...
     Found 99500 records...
     Found 1e+05 records...
     Found 100500 records...
     Found 101000 records...
     Found 101500 records...
     Found 101904 records...
     Imported 101904 records. Simplifying...

    closing file input connection.

``` r
head(json_data)
```

                    @timestamp @metadata.beat @metadata.type @metadata.version
    1 2019-10-20T20:11:06.937Z     winlogbeat           _doc             7.4.0
    2 2019-10-20T20:11:07.101Z     winlogbeat           _doc             7.4.0
    3 2019-10-20T20:11:09.052Z     winlogbeat           _doc             7.4.0
    4 2019-10-20T20:11:10.985Z     winlogbeat           _doc             7.4.0
    5 2019-10-20T20:11:11.249Z     winlogbeat           _doc             7.4.0
    6 2019-10-20T20:11:15.017Z     winlogbeat           _doc             7.4.0
      @metadata.topic            event.created event.kind event.code
    1      winlogbeat 2019-10-20T20:11:09.988Z      event       4703
    2      winlogbeat 2019-10-20T20:11:09.988Z      event       4673
    3      winlogbeat 2019-10-20T20:11:11.995Z      event         10
    4      winlogbeat 2019-10-20T20:11:14.013Z      event         10
    5      winlogbeat 2019-10-20T20:11:14.013Z      event         10
    6      winlogbeat 2019-10-20T20:11:18.027Z      event         10
                                event.action       level
    1            Token Right Adjusted Events information
    2                Sensitive Privilege Use information
    3 Process accessed (rule: ProcessAccess) information
    4 Process accessed (rule: ProcessAccess) information
    5 Process accessed (rule: ProcessAccess) information
    6 Process accessed (rule: ProcessAccess) information
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        message
    1                                                                                                                                                                                                                                                                                                                                                                                                               A token right was adjusted.\n\nSubject:\n\tSecurity ID:\t\tS-1-5-18\n\tAccount Name:\t\tHR001$\n\tAccount Domain:\t\tshire\n\tLogon ID:\t\t0x3E7\n\nTarget Account:\n\tSecurity ID:\t\tS-1-5-18\n\tAccount Name:\t\tHR001$\n\tAccount Domain:\t\tshire\n\tLogon ID:\t\t0x3E7\n\nProcess Information:\n\tProcess ID:\t\t0x804\n\tProcess Name:\t\tC:\\Windows\\System32\\svchost.exe\n\nEnabled Privileges:\n\t\t\tSeTakeOwnershipPrivilege\n\nDisabled Privileges:\n\t\t\t-
    2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         A privileged service was called.\n\nSubject:\n\tSecurity ID:\t\tS-1-5-19\n\tAccount Name:\t\tLOCAL SERVICE\n\tAccount Domain:\t\tNT AUTHORITY\n\tLogon ID:\t\t0x3E5\n\nService:\n\tServer:\tSecurity\n\tService Name:\t-\n\nProcess:\n\tProcess ID:\t0x494\n\tProcess Name:\tC:\\Windows\\System32\\svchost.exe\n\nService Request Information:\n\tPrivileges:\t\tSeProfileSingleProcessPrivilege
    3                                            Process accessed:\nRuleName: \nUtcTime: 2019-10-20 20:11:09.052\nSourceProcessGUID: {a158f72c-afec-5dac-0000-001030640200}\nSourceProcessId: 3556\nSourceThreadId: 3688\nSourceImage: C:\\Windows\\System32\\svchost.exe\nTargetProcessGUID: {a158f72c-afeb-5dac-0000-001082220200}\nTargetProcessId: 3108\nTargetImage: C:\\Program Files\\Amazon\\Ec2ConfigService\\Ec2Config.exe\nGrantedAccess: 0x1400\nCallTrace: C:\\Windows\\SYSTEM32\\ntdll.dll+9c524|C:\\Windows\\System32\\KERNELBASE.dll+2730e|c:\\windows\\system32\\rasmans.dll+3751b|c:\\windows\\system32\\rasmans.dll+36fad|c:\\windows\\system32\\rasmans.dll+10ed8|c:\\windows\\system32\\rasmans.dll+33cd5|C:\\Windows\\System32\\svchost.exe+314c|C:\\Windows\\System32\\sechost.dll+2de2|C:\\Windows\\System32\\KERNEL32.DLL+17944|C:\\Windows\\SYSTEM32\\ntdll.dll+6ce71
    4                                            Process accessed:\nRuleName: \nUtcTime: 2019-10-20 20:11:10.985\nSourceProcessGUID: {a158f72c-afe7-5dac-0000-0010044f0200}\nSourceProcessId: 3548\nSourceThreadId: 3660\nSourceImage: C:\\Windows\\System32\\svchost.exe\nTargetProcessGUID: {a158f72c-afe6-5dac-0000-001077030200}\nTargetProcessId: 1968\nTargetImage: C:\\Program Files\\Amazon\\Ec2ConfigService\\Ec2Config.exe\nGrantedAccess: 0x1400\nCallTrace: C:\\Windows\\SYSTEM32\\ntdll.dll+9c524|C:\\Windows\\System32\\KERNELBASE.dll+2730e|c:\\windows\\system32\\rasmans.dll+3751b|c:\\windows\\system32\\rasmans.dll+36fad|c:\\windows\\system32\\rasmans.dll+10ed8|c:\\windows\\system32\\rasmans.dll+33cd5|C:\\Windows\\System32\\svchost.exe+314c|C:\\Windows\\System32\\sechost.dll+2de2|C:\\Windows\\System32\\KERNEL32.DLL+17944|C:\\Windows\\SYSTEM32\\ntdll.dll+6ce71
    5                                            Process accessed:\nRuleName: \nUtcTime: 2019-10-20 20:11:11.249\nSourceProcessGUID: {a158f72c-afe6-5dac-0000-00107a640200}\nSourceProcessId: 3632\nSourceThreadId: 3772\nSourceImage: C:\\Windows\\System32\\svchost.exe\nTargetProcessGUID: {a158f72c-afe4-5dac-0000-00104d040200}\nTargetProcessId: 2032\nTargetImage: C:\\Program Files\\Amazon\\Ec2ConfigService\\Ec2Config.exe\nGrantedAccess: 0x1400\nCallTrace: C:\\Windows\\SYSTEM32\\ntdll.dll+9c524|C:\\Windows\\System32\\KERNELBASE.dll+2730e|c:\\windows\\system32\\rasmans.dll+3751b|c:\\windows\\system32\\rasmans.dll+36fad|c:\\windows\\system32\\rasmans.dll+10ed8|c:\\windows\\system32\\rasmans.dll+33cd5|C:\\Windows\\System32\\svchost.exe+314c|C:\\Windows\\System32\\sechost.dll+2de2|C:\\Windows\\System32\\KERNEL32.DLL+17944|C:\\Windows\\SYSTEM32\\ntdll.dll+6ce71
    6 Process accessed:\nRuleName: \nUtcTime: 2019-10-20 20:11:15.016\nSourceProcessGUID: {a158f72c-b09e-5dac-0000-0010555c2c00}\nSourceProcessId: 7112\nSourceThreadId: 7004\nSourceImage: C:\\ProgramData\\Microsoft\\Windows Defender\\platform\\4.18.1909.6-0\\MsMpEng.exe\nTargetProcessGUID: {a158f72c-b0a0-5dac-0000-0010810f2d00}\nTargetProcessId: 7592\nTargetImage: C:\\ProgramData\\Microsoft\\Windows Defender\\platform\\4.18.1909.6-0\\NisSrv.exe\nGrantedAccess: 0x1400\nCallTrace: C:\\Windows\\SYSTEM32\\ntdll.dll+9c524|C:\\Windows\\System32\\KERNELBASE.dll+2730e|C:\\ProgramData\\Microsoft\\Windows Defender\\platform\\4.18.1909.6-0\\mpsvc.dll+db61b|C:\\ProgramData\\Microsoft\\Windows Defender\\platform\\4.18.1909.6-0\\mpsvc.dll+dec42|C:\\Windows\\System32\\ucrtbase.dll+1d912|C:\\Windows\\System32\\KERNEL32.DLL+17944|C:\\Windows\\SYSTEM32\\ntdll.dll+6ce71
      winlog.event_data.SubjectDomainName winlog.event_data.TargetDomainName
    1                               shire                              shire
    2                        NT AUTHORITY                               <NA>
    3                                <NA>                               <NA>
    4                                <NA>                               <NA>
    5                                <NA>                               <NA>
    6                                <NA>                               <NA>
      winlog.event_data.SubjectUserSid winlog.event_data.SubjectUserName
    1                         S-1-5-18                            HR001$
    2                         S-1-5-19                     LOCAL SERVICE
    3                             <NA>                              <NA>
    4                             <NA>                              <NA>
    5                             <NA>                              <NA>
    6                             <NA>                              <NA>
      winlog.event_data.TargetUserName winlog.event_data.EnabledPrivilegeList
    1                           HR001$               SeTakeOwnershipPrivilege
    2                             <NA>                                   <NA>
    3                             <NA>                                   <NA>
    4                             <NA>                                   <NA>
    5                             <NA>                                   <NA>
    6                             <NA>                                   <NA>
      winlog.event_data.TargetLogonId      winlog.event_data.ProcessName
    1                           0x3e7 C:\\Windows\\System32\\svchost.exe
    2                            <NA> C:\\Windows\\System32\\svchost.exe
    3                            <NA>                               <NA>
    4                            <NA>                               <NA>
    5                            <NA>                               <NA>
    6                            <NA>                               <NA>
      winlog.event_data.ProcessId winlog.event_data.SubjectLogonId
    1                       0x804                            0x3e7
    2                       0x494                            0x3e5
    3                        <NA>                             <NA>
    4                        <NA>                             <NA>
    5                        <NA>                             <NA>
    6                        <NA>                             <NA>
      winlog.event_data.TargetUserSid winlog.event_data.DisabledPrivilegeList
    1                        S-1-5-18                                       -
    2                            <NA>                                    <NA>
    3                            <NA>                                    <NA>
    4                            <NA>                                    <NA>
    5                            <NA>                                    <NA>
    6                            <NA>                                    <NA>
      winlog.event_data.ObjectServer winlog.event_data.Service
    1                           <NA>                      <NA>
    2                       Security                         -
    3                           <NA>                      <NA>
    4                           <NA>                      <NA>
    5                           <NA>                      <NA>
    6                           <NA>                      <NA>
      winlog.event_data.PrivilegeList winlog.event_data.TargetProcessId
    1                            <NA>                              <NA>
    2 SeProfileSingleProcessPrivilege                              <NA>
    3                            <NA>                              3108
    4                            <NA>                              1968
    5                            <NA>                              2032
    6                            <NA>                              7592
      winlog.event_data.SourceProcessId    winlog.event_data.SourceProcessGUID
    1                              <NA>                                   <NA>
    2                              <NA>                                   <NA>
    3                              3556 {a158f72c-afec-5dac-0000-001030640200}
    4                              3548 {a158f72c-afe7-5dac-0000-0010044f0200}
    5                              3632 {a158f72c-afe6-5dac-0000-00107a640200}
    6                              7112 {a158f72c-b09e-5dac-0000-0010555c2c00}
      winlog.event_data.SourceThreadId
    1                             <NA>
    2                             <NA>
    3                             3688
    4                             3660
    5                             3772
    6                             7004
                                                           winlog.event_data.SourceImage
    1                                                                               <NA>
    2                                                                               <NA>
    3                                                 C:\\Windows\\System32\\svchost.exe
    4                                                 C:\\Windows\\System32\\svchost.exe
    5                                                 C:\\Windows\\System32\\svchost.exe
    6 C:\\ProgramData\\Microsoft\\Windows Defender\\platform\\4.18.1909.6-0\\MsMpEng.exe
                                                                                                                                                                                                                                                                                                                                                                                                  winlog.event_data.CallTrace
    1                                                                                                                                                                                                                                                                                                                                                                                                                    <NA>
    2                                                                                                                                                                                                                                                                                                                                                                                                                    <NA>
    3 C:\\Windows\\SYSTEM32\\ntdll.dll+9c524|C:\\Windows\\System32\\KERNELBASE.dll+2730e|c:\\windows\\system32\\rasmans.dll+3751b|c:\\windows\\system32\\rasmans.dll+36fad|c:\\windows\\system32\\rasmans.dll+10ed8|c:\\windows\\system32\\rasmans.dll+33cd5|C:\\Windows\\System32\\svchost.exe+314c|C:\\Windows\\System32\\sechost.dll+2de2|C:\\Windows\\System32\\KERNEL32.DLL+17944|C:\\Windows\\SYSTEM32\\ntdll.dll+6ce71
    4 C:\\Windows\\SYSTEM32\\ntdll.dll+9c524|C:\\Windows\\System32\\KERNELBASE.dll+2730e|c:\\windows\\system32\\rasmans.dll+3751b|c:\\windows\\system32\\rasmans.dll+36fad|c:\\windows\\system32\\rasmans.dll+10ed8|c:\\windows\\system32\\rasmans.dll+33cd5|C:\\Windows\\System32\\svchost.exe+314c|C:\\Windows\\System32\\sechost.dll+2de2|C:\\Windows\\System32\\KERNEL32.DLL+17944|C:\\Windows\\SYSTEM32\\ntdll.dll+6ce71
    5 C:\\Windows\\SYSTEM32\\ntdll.dll+9c524|C:\\Windows\\System32\\KERNELBASE.dll+2730e|c:\\windows\\system32\\rasmans.dll+3751b|c:\\windows\\system32\\rasmans.dll+36fad|c:\\windows\\system32\\rasmans.dll+10ed8|c:\\windows\\system32\\rasmans.dll+33cd5|C:\\Windows\\System32\\svchost.exe+314c|C:\\Windows\\System32\\sechost.dll+2de2|C:\\Windows\\System32\\KERNEL32.DLL+17944|C:\\Windows\\SYSTEM32\\ntdll.dll+6ce71
    6                             C:\\Windows\\SYSTEM32\\ntdll.dll+9c524|C:\\Windows\\System32\\KERNELBASE.dll+2730e|C:\\ProgramData\\Microsoft\\Windows Defender\\platform\\4.18.1909.6-0\\mpsvc.dll+db61b|C:\\ProgramData\\Microsoft\\Windows Defender\\platform\\4.18.1909.6-0\\mpsvc.dll+dec42|C:\\Windows\\System32\\ucrtbase.dll+1d912|C:\\Windows\\System32\\KERNEL32.DLL+17944|C:\\Windows\\SYSTEM32\\ntdll.dll+6ce71
         winlog.event_data.TargetProcessGUID
    1                                   <NA>
    2                                   <NA>
    3 {a158f72c-afeb-5dac-0000-001082220200}
    4 {a158f72c-afe6-5dac-0000-001077030200}
    5 {a158f72c-afe4-5dac-0000-00104d040200}
    6 {a158f72c-b0a0-5dac-0000-0010810f2d00}
                                                          winlog.event_data.TargetImage
    1                                                                              <NA>
    2                                                                              <NA>
    3                        C:\\Program Files\\Amazon\\Ec2ConfigService\\Ec2Config.exe
    4                        C:\\Program Files\\Amazon\\Ec2ConfigService\\Ec2Config.exe
    5                        C:\\Program Files\\Amazon\\Ec2ConfigService\\Ec2Config.exe
    6 C:\\ProgramData\\Microsoft\\Windows Defender\\platform\\4.18.1909.6-0\\NisSrv.exe
      winlog.event_data.GrantedAccess winlog.event_data.UtcTime
    1                            <NA>                      <NA>
    2                            <NA>                      <NA>
    3                          0x1400   2019-10-20 20:11:09.052
    4                          0x1400   2019-10-20 20:11:10.985
    5                          0x1400   2019-10-20 20:11:11.249
    6                          0x1400   2019-10-20 20:11:15.016
      winlog.event_data.ProcessGuid winlog.event_data.Image
    1                          <NA>                    <NA>
    2                          <NA>                    <NA>
    3                          <NA>                    <NA>
    4                          <NA>                    <NA>
    5                          <NA>                    <NA>
    6                          <NA>                    <NA>
      winlog.event_data.TargetFilename winlog.event_data.CreationUtcTime
    1                             <NA>                              <NA>
    2                             <NA>                              <NA>
    3                             <NA>                              <NA>
    4                             <NA>                              <NA>
    5                             <NA>                              <NA>
    6                             <NA>                              <NA>
      winlog.event_data.FileVersion winlog.event_data.Company
    1                          <NA>                      <NA>
    2                          <NA>                      <NA>
    3                          <NA>                      <NA>
    4                          <NA>                      <NA>
    5                          <NA>                      <NA>
    6                          <NA>                      <NA>
      winlog.event_data.Signed winlog.event_data.Signature
    1                     <NA>                        <NA>
    2                     <NA>                        <NA>
    3                     <NA>                        <NA>
    4                     <NA>                        <NA>
    5                     <NA>                        <NA>
    6                     <NA>                        <NA>
      winlog.event_data.OriginalFileName winlog.event_data.Description
    1                               <NA>                          <NA>
    2                               <NA>                          <NA>
    3                               <NA>                          <NA>
    4                               <NA>                          <NA>
    5                               <NA>                          <NA>
    6                               <NA>                          <NA>
      winlog.event_data.Product winlog.event_data.ImageLoaded
    1                      <NA>                          <NA>
    2                      <NA>                          <NA>
    3                      <NA>                          <NA>
    4                      <NA>                          <NA>
    5                      <NA>                          <NA>
    6                      <NA>                          <NA>
      winlog.event_data.SignatureStatus winlog.event_data.Hashes
    1                              <NA>                     <NA>
    2                              <NA>                     <NA>
    3                              <NA>                     <NA>
    4                              <NA>                     <NA>
    5                              <NA>                     <NA>
    6                              <NA>                     <NA>
      winlog.event_data.Status winlog.event_data.Protocol
    1                     <NA>                       <NA>
    2                     <NA>                       <NA>
    3                     <NA>                       <NA>
    4                     <NA>                       <NA>
    5                     <NA>                       <NA>
    6                     <NA>                       <NA>
      winlog.event_data.FilterRTID winlog.event_data.LayerName
    1                         <NA>                        <NA>
    2                         <NA>                        <NA>
    3                         <NA>                        <NA>
    4                         <NA>                        <NA>
    5                         <NA>                        <NA>
    6                         <NA>                        <NA>
      winlog.event_data.LayerRTID winlog.event_data.Application
    1                        <NA>                          <NA>
    2                        <NA>                          <NA>
    3                        <NA>                          <NA>
    4                        <NA>                          <NA>
    5                        <NA>                          <NA>
    6                        <NA>                          <NA>
      winlog.event_data.SourceAddress winlog.event_data.SourcePort
    1                            <NA>                         <NA>
    2                            <NA>                         <NA>
    3                            <NA>                         <NA>
    4                            <NA>                         <NA>
    5                            <NA>                         <NA>
    6                            <NA>                         <NA>
      winlog.event_data.ProcessID winlog.event_data.DestPort
    1                        <NA>                       <NA>
    2                        <NA>                       <NA>
    3                        <NA>                       <NA>
    4                        <NA>                       <NA>
    5                        <NA>                       <NA>
    6                        <NA>                       <NA>
      winlog.event_data.RemoteMachineID winlog.event_data.RemoteUserID
    1                              <NA>                           <NA>
    2                              <NA>                           <NA>
    3                              <NA>                           <NA>
    4                              <NA>                           <NA>
    5                              <NA>                           <NA>
    6                              <NA>                           <NA>
      winlog.event_data.Direction winlog.event_data.DestAddress
    1                        <NA>                          <NA>
    2                        <NA>                          <NA>
    3                        <NA>                          <NA>
    4                        <NA>                          <NA>
    5                        <NA>                          <NA>
    6                        <NA>                          <NA>
      winlog.event_data.RestrictedAdminMode winlog.event_data.TransmittedServices
    1                                  <NA>                                  <NA>
    2                                  <NA>                                  <NA>
    3                                  <NA>                                  <NA>
    4                                  <NA>                                  <NA>
    5                                  <NA>                                  <NA>
    6                                  <NA>                                  <NA>
      winlog.event_data.LogonType winlog.event_data.WorkstationName
    1                        <NA>                              <NA>
    2                        <NA>                              <NA>
    3                        <NA>                              <NA>
    4                        <NA>                              <NA>
    5                        <NA>                              <NA>
    6                        <NA>                              <NA>
      winlog.event_data.LmPackageName winlog.event_data.KeyLength
    1                            <NA>                        <NA>
    2                            <NA>                        <NA>
    3                            <NA>                        <NA>
    4                            <NA>                        <NA>
    5                            <NA>                        <NA>
    6                            <NA>                        <NA>
      winlog.event_data.LogonGuid winlog.event_data.ImpersonationLevel
    1                        <NA>                                 <NA>
    2                        <NA>                                 <NA>
    3                        <NA>                                 <NA>
    4                        <NA>                                 <NA>
    5                        <NA>                                 <NA>
    6                        <NA>                                 <NA>
      winlog.event_data.TargetOutboundDomainName
    1                                       <NA>
    2                                       <NA>
    3                                       <NA>
    4                                       <NA>
    5                                       <NA>
    6                                       <NA>
      winlog.event_data.TargetLinkedLogonId
    1                                  <NA>
    2                                  <NA>
    3                                  <NA>
    4                                  <NA>
    5                                  <NA>
    6                                  <NA>
      winlog.event_data.AuthenticationPackageName
    1                                        <NA>
    2                                        <NA>
    3                                        <NA>
    4                                        <NA>
    5                                        <NA>
    6                                        <NA>
      winlog.event_data.TargetOutboundUserName winlog.event_data.VirtualAccount
    1                                     <NA>                             <NA>
    2                                     <NA>                             <NA>
    3                                     <NA>                             <NA>
    4                                     <NA>                             <NA>
    5                                     <NA>                             <NA>
    6                                     <NA>                             <NA>
      winlog.event_data.IpAddress winlog.event_data.ElevatedToken
    1                        <NA>                            <NA>
    2                        <NA>                            <NA>
    3                        <NA>                            <NA>
    4                        <NA>                            <NA>
    5                        <NA>                            <NA>
    6                        <NA>                            <NA>
      winlog.event_data.LogonProcessName winlog.event_data.IpPort
    1                               <NA>                     <NA>
    2                               <NA>                     <NA>
    3                               <NA>                     <NA>
    4                               <NA>                     <NA>
    5                               <NA>                     <NA>
    6                               <NA>                     <NA>
      winlog.event_data.GroupMembership winlog.event_data.EventIdx
    1                              <NA>                       <NA>
    2                              <NA>                       <NA>
    3                              <NA>                       <NA>
    4                              <NA>                       <NA>
    5                              <NA>                       <NA>
    6                              <NA>                       <NA>
      winlog.event_data.EventCountTotal winlog.event_data.EventType
    1                              <NA>                        <NA>
    2                              <NA>                        <NA>
    3                              <NA>                        <NA>
    4                              <NA>                        <NA>
    5                              <NA>                        <NA>
    6                              <NA>                        <NA>
      winlog.event_data.TargetObject winlog.event_data.DestinationPort
    1                           <NA>                              <NA>
    2                           <NA>                              <NA>
    3                           <NA>                              <NA>
    4                           <NA>                              <NA>
    5                           <NA>                              <NA>
    6                           <NA>                              <NA>
      winlog.event_data.DestinationIsIpv6 winlog.event_data.SourceIp
    1                                <NA>                       <NA>
    2                                <NA>                       <NA>
    3                                <NA>                       <NA>
    4                                <NA>                       <NA>
    5                                <NA>                       <NA>
    6                                <NA>                       <NA>
      winlog.event_data.User winlog.event_data.SourceIsIpv6
    1                   <NA>                           <NA>
    2                   <NA>                           <NA>
    3                   <NA>                           <NA>
    4                   <NA>                           <NA>
    5                   <NA>                           <NA>
    6                   <NA>                           <NA>
      winlog.event_data.DestinationIp winlog.event_data.SourceHostname
    1                            <NA>                             <NA>
    2                            <NA>                             <NA>
    3                            <NA>                             <NA>
    4                            <NA>                             <NA>
    5                            <NA>                             <NA>
    6                            <NA>                             <NA>
      winlog.event_data.DestinationHostname winlog.event_data.DestinationPortName
    1                                  <NA>                                  <NA>
    2                                  <NA>                                  <NA>
    3                                  <NA>                                  <NA>
    4                                  <NA>                                  <NA>
    5                                  <NA>                                  <NA>
    6                                  <NA>                                  <NA>
      winlog.event_data.Initiated winlog.event_data.param3 winlog.event_data.param2
    1                        <NA>                     <NA>                     <NA>
    2                        <NA>                     <NA>                     <NA>
    3                        <NA>                     <NA>                     <NA>
    4                        <NA>                     <NA>                     <NA>
    5                        <NA>                     <NA>                     <NA>
    6                        <NA>                     <NA>                     <NA>
      winlog.event_data.param1 winlog.event_data.ParentProcessId
    1                     <NA>                              <NA>
    2                     <NA>                              <NA>
    3                     <NA>                              <NA>
    4                     <NA>                              <NA>
    5                     <NA>                              <NA>
    6                     <NA>                              <NA>
      winlog.event_data.ParentProcessGuid winlog.event_data.ParentCommandLine
    1                                <NA>                                <NA>
    2                                <NA>                                <NA>
    3                                <NA>                                <NA>
    4                                <NA>                                <NA>
    5                                <NA>                                <NA>
    6                                <NA>                                <NA>
      winlog.event_data.CommandLine winlog.event_data.CurrentDirectory
    1                          <NA>                               <NA>
    2                          <NA>                               <NA>
    3                          <NA>                               <NA>
    4                          <NA>                               <NA>
    5                          <NA>                               <NA>
    6                          <NA>                               <NA>
      winlog.event_data.TerminalSessionId winlog.event_data.ParentImage
    1                                <NA>                          <NA>
    2                                <NA>                          <NA>
    3                                <NA>                          <NA>
    4                                <NA>                          <NA>
    5                                <NA>                          <NA>
    6                                <NA>                          <NA>
      winlog.event_data.LogonId winlog.event_data.IntegrityLevel
    1                      <NA>                             <NA>
    2                      <NA>                             <NA>
    3                      <NA>                             <NA>
    4                      <NA>                             <NA>
    5                      <NA>                             <NA>
    6                      <NA>                             <NA>
      winlog.event_data.PipeName winlog.event_data.ScriptBlockId
    1                       <NA>                            <NA>
    2                       <NA>                            <NA>
    3                       <NA>                            <NA>
    4                       <NA>                            <NA>
    5                       <NA>                            <NA>
    6                       <NA>                            <NA>
      winlog.event_data.RunspaceId winlog.event_data.ContextInfo
    1                         <NA>                          <NA>
    2                         <NA>                          <NA>
    3                         <NA>                          <NA>
    4                         <NA>                          <NA>
    5                         <NA>                          <NA>
    6                         <NA>                          <NA>
      winlog.event_data.Payload winlog.event_data.MessageNumber
    1                      <NA>                            <NA>
    2                      <NA>                            <NA>
    3                      <NA>                            <NA>
    4                      <NA>                            <NA>
    5                      <NA>                            <NA>
    6                      <NA>                            <NA>
      winlog.event_data.MessageTotal winlog.event_data.ScriptBlockText
    1                           <NA>                              <NA>
    2                           <NA>                              <NA>
    3                           <NA>                              <NA>
    4                           <NA>                              <NA>
    5                           <NA>                              <NA>
    6                           <NA>                              <NA>
      winlog.event_data.NewProcessId winlog.event_data.NewProcessName
    1                           <NA>                             <NA>
    2                           <NA>                             <NA>
    3                           <NA>                             <NA>
    4                           <NA>                             <NA>
    5                           <NA>                             <NA>
    6                           <NA>                             <NA>
      winlog.event_data.MandatoryLabel winlog.event_data.ParentProcessName
    1                             <NA>                                <NA>
    2                             <NA>                                <NA>
    3                             <NA>                                <NA>
    4                             <NA>                                <NA>
    5                             <NA>                                <NA>
    6                             <NA>                                <NA>
      winlog.event_data.TokenElevationType winlog.event_data.ServiceName
    1                                 <NA>                          <NA>
    2                                 <NA>                          <NA>
    3                                 <NA>                          <NA>
    4                                 <NA>                          <NA>
    5                                 <NA>                          <NA>
    6                                 <NA>                          <NA>
      winlog.event_data.ServiceSid winlog.event_data.TicketOptions
    1                         <NA>                            <NA>
    2                         <NA>                            <NA>
    3                         <NA>                            <NA>
    4                         <NA>                            <NA>
    5                         <NA>                            <NA>
    6                         <NA>                            <NA>
      winlog.event_data.TicketEncryptionType winlog.event_data.QueryName
    1                                   <NA>                        <NA>
    2                                   <NA>                        <NA>
    3                                   <NA>                        <NA>
    4                                   <NA>                        <NA>
    5                                   <NA>                        <NA>
    6                                   <NA>                        <NA>
      winlog.event_data.QueryStatus winlog.event_data.QueryResults
    1                          <NA>                           <NA>
    2                          <NA>                           <NA>
    3                          <NA>                           <NA>
    4                          <NA>                           <NA>
    5                          <NA>                           <NA>
    6                          <NA>                           <NA>
      winlog.event_data.SourcePortName winlog.event_data.Device
    1                             <NA>                     <NA>
    2                             <NA>                     <NA>
    3                             <NA>                     <NA>
    4                             <NA>                     <NA>
    5                             <NA>                     <NA>
    6                             <NA>                     <NA>
      winlog.event_data.Details winlog.event_data.ObjectName
    1                      <NA>                         <NA>
    2                      <NA>                         <NA>
    3                      <NA>                         <NA>
    4                      <NA>                         <NA>
    5                      <NA>                         <NA>
    6                      <NA>                         <NA>
      winlog.event_data.OldSd winlog.event_data.NewSd winlog.event_data.ObjectType
    1                    <NA>                    <NA>                         <NA>
    2                    <NA>                    <NA>                         <NA>
    3                    <NA>                    <NA>                         <NA>
    4                    <NA>                    <NA>                         <NA>
    5                    <NA>                    <NA>                         <NA>
    6                    <NA>                    <NA>                         <NA>
      winlog.event_data.HandleId winlog.event_data.ShareName
    1                       <NA>                        <NA>
    2                       <NA>                        <NA>
    3                       <NA>                        <NA>
    4                       <NA>                        <NA>
    5                       <NA>                        <NA>
    6                       <NA>                        <NA>
      winlog.event_data.AccessList winlog.event_data.ShareLocalPath
    1                         <NA>                             <NA>
    2                         <NA>                             <NA>
    3                         <NA>                             <NA>
    4                         <NA>                             <NA>
    5                         <NA>                             <NA>
    6                         <NA>                             <NA>
      winlog.event_data.AccessMask winlog.event_data.AccessReason
    1                         <NA>                           <NA>
    2                         <NA>                           <NA>
    3                         <NA>                           <NA>
    4                         <NA>                           <NA>
    5                         <NA>                           <NA>
    6                         <NA>                           <NA>
      winlog.event_data.RelativeTargetName winlog.event_data.Properties
    1                                 <NA>                         <NA>
    2                                 <NA>                         <NA>
    3                                 <NA>                         <NA>
    4                                 <NA>                         <NA>
    5                                 <NA>                         <NA>
    6                                 <NA>                         <NA>
      winlog.event_data.AdditionalInfo2 winlog.event_data.OperationType
    1                              <NA>                            <NA>
    2                              <NA>                            <NA>
    3                              <NA>                            <NA>
    4                              <NA>                            <NA>
    5                              <NA>                            <NA>
    6                              <NA>                            <NA>
      winlog.event_data.AdditionalInfo winlog.event_data.Path
    1                             <NA>                   <NA>
    2                             <NA>                   <NA>
    3                             <NA>                   <NA>
    4                             <NA>                   <NA>
    5                             <NA>                   <NA>
    6                             <NA>                   <NA>
      winlog.event_data.TargetHandleId winlog.event_data.SourceHandleId
    1                             <NA>                             <NA>
    2                             <NA>                             <NA>
    3                             <NA>                             <NA>
    4                             <NA>                             <NA>
    5                             <NA>                             <NA>
    6                             <NA>                             <NA>
      winlog.event_data.TransactionId winlog.event_data.RestrictedSidCount
    1                            <NA>                                 <NA>
    2                            <NA>                                 <NA>
    3                            <NA>                                 <NA>
    4                            <NA>                                 <NA>
    5                            <NA>                                 <NA>
    6                            <NA>                                 <NA>
      winlog.event_data.ResourceAttributes winlog.event_data.Binary
    1                                 <NA>                     <NA>
    2                                 <NA>                     <NA>
    3                                 <NA>                     <NA>
    4                                 <NA>                     <NA>
    5                                 <NA>                     <NA>
    6                                 <NA>                     <NA>
      winlog.event_data.PreviousCreationUtcTime winlog.event_data.CallerProcessId
    1                                      <NA>                              <NA>
    2                                      <NA>                              <NA>
    3                                      <NA>                              <NA>
    4                                      <NA>                              <NA>
    5                                      <NA>                              <NA>
    6                                      <NA>                              <NA>
      winlog.event_data.CallerProcessName winlog.event_data.TargetSid
    1                                <NA>                        <NA>
    2                                <NA>                        <NA>
    3                                <NA>                        <NA>
    4                                <NA>                        <NA>
    5                                <NA>                        <NA>
    6                                <NA>                        <NA>
      winlog.event_data.TaskName winlog.event_data.TaskContentNew
    1                       <NA>                             <NA>
    2                       <NA>                             <NA>
    3                       <NA>                             <NA>
    4                       <NA>                             <NA>
    5                       <NA>                             <NA>
    6                       <NA>                             <NA>
      winlog.event_data.SourceProcessGuid winlog.event_data.TargetProcessGuid
    1                                <NA>                                <NA>
    2                                <NA>                                <NA>
    3                                <NA>                                <NA>
    4                                <NA>                                <NA>
    5                                <NA>                                <NA>
    6                                <NA>                                <NA>
      winlog.event_data.StartAddress winlog.event_data.NewThreadId
    1                           <NA>                          <NA>
    2                           <NA>                          <NA>
    3                           <NA>                          <NA>
    4                           <NA>                          <NA>
    5                           <NA>                          <NA>
    6                           <NA>                          <NA>
      winlog.event_data.TaskContent winlog.event_data.PackageName
    1                          <NA>                          <NA>
    2                          <NA>                          <NA>
    3                          <NA>                          <NA>
    4                          <NA>                          <NA>
    5                          <NA>                          <NA>
    6                          <NA>                          <NA>
      winlog.event_data.Workstation winlog.event_data.param4
    1                          <NA>                     <NA>
    2                          <NA>                     <NA>
    3                          <NA>                     <NA>
    4                          <NA>                     <NA>
    5                          <NA>                     <NA>
    6                          <NA>                     <NA>
      winlog.event_data.param5 winlog.event_data.param7
    1                     <NA>                     <NA>
    2                     <NA>                     <NA>
    3                     <NA>                     <NA>
    4                     <NA>                     <NA>
    5                     <NA>                     <NA>
    6                     <NA>                     <NA>
      winlog.event_data.StartModule winlog.event_data.StartFunction
    1                          <NA>                            <NA>
    2                          <NA>                            <NA>
    3                          <NA>                            <NA>
    4                          <NA>                            <NA>
    5                          <NA>                            <NA>
    6                          <NA>                            <NA>
      winlog.event_data.TSId winlog.event_data.UserSid
    1                   <NA>                      <NA>
    2                   <NA>                      <NA>
    3                   <NA>                      <NA>
    4                   <NA>                      <NA>
    5                   <NA>                      <NA>
    6                   <NA>                      <NA>
      winlog.event_data.PreAuthType winlog.event_data.PreviousTime
    1                          <NA>                           <NA>
    2                          <NA>                           <NA>
    3                          <NA>                           <NA>
    4                          <NA>                           <NA>
    5                          <NA>                           <NA>
    6                          <NA>                           <NA>
      winlog.event_data.NewTime winlog.event_data.InterfaceName
    1                      <NA>                            <NA>
    2                      <NA>                            <NA>
    3                      <NA>                            <NA>
    4                      <NA>                            <NA>
    5                      <NA>                            <NA>
    6                      <NA>                            <NA>
      winlog.event_data.OldProfile winlog.event_data.NewProfile
    1                         <NA>                         <NA>
    2                         <NA>                         <NA>
    3                         <NA>                         <NA>
    4                         <NA>                         <NA>
    5                         <NA>                         <NA>
    6                         <NA>                         <NA>
      winlog.event_data.InterfaceGuid winlog.event_data.SettingType
    1                            <NA>                          <NA>
    2                            <NA>                          <NA>
    3                            <NA>                          <NA>
    4                            <NA>                          <NA>
    5                            <NA>                          <NA>
    6                            <NA>                          <NA>
      winlog.event_data.SettingValueSize winlog.event_data.SettingValue
    1                               <NA>                           <NA>
    2                               <NA>                           <NA>
    3                               <NA>                           <NA>
    4                               <NA>                           <NA>
    5                               <NA>                           <NA>
    6                               <NA>                           <NA>
      winlog.event_data.SettingValueDisplay winlog.event_data.Origin
    1                                  <NA>                     <NA>
    2                                  <NA>                     <NA>
    3                                  <NA>                     <NA>
    4                                  <NA>                     <NA>
    5                                  <NA>                     <NA>
    6                                  <NA>                     <NA>
      winlog.event_data.ModifyingUser winlog.event_data.Reason
    1                            <NA>                     <NA>
    2                            <NA>                     <NA>
    3                            <NA>                     <NA>
    4                            <NA>                     <NA>
    5                            <NA>                     <NA>
    6                            <NA>                     <NA>
      winlog.event_data.OldTime winlog.event_data.DwordVal winlog.event_data.param6
    1                      <NA>                       <NA>                     <NA>
    2                      <NA>                       <NA>                     <NA>
    3                      <NA>                       <NA>                     <NA>
    4                      <NA>                       <NA>                     <NA>
    5                      <NA>                       <NA>                     <NA>
    6                      <NA>                       <NA>                     <NA>
      winlog.event_data.ShutdownActionType winlog.event_data.ShutdownEventCode
    1                                 <NA>                                <NA>
    2                                 <NA>                                <NA>
    3                                 <NA>                                <NA>
    4                                 <NA>                                <NA>
    5                                 <NA>                                <NA>
    6                                 <NA>                                <NA>
      winlog.event_data.ShutdownReason winlog.event_data.StopTime
    1                             <NA>                       <NA>
    2                             <NA>                       <NA>
    3                             <NA>                       <NA>
    4                             <NA>                       <NA>
    5                             <NA>                       <NA>
    6                             <NA>                       <NA>
      winlog.event_data.BootMode winlog.event_data.StartTime
    1                       <NA>                        <NA>
    2                       <NA>                        <NA>
    3                       <NA>                        <NA>
    4                       <NA>                        <NA>
    5                       <NA>                        <NA>
    6                       <NA>                        <NA>
      winlog.event_data.MajorVersion winlog.event_data.MinorVersion
    1                           <NA>                           <NA>
    2                           <NA>                           <NA>
    3                           <NA>                           <NA>
    4                           <NA>                           <NA>
    5                           <NA>                           <NA>
    6                           <NA>                           <NA>
      winlog.event_data.BuildVersion winlog.event_data.QfeVersion
    1                           <NA>                         <NA>
    2                           <NA>                         <NA>
    3                           <NA>                         <NA>
    4                           <NA>                         <NA>
    5                           <NA>                         <NA>
    6                           <NA>                         <NA>
      winlog.event_data.ServiceVersion winlog.event_data.EnableDisableReason
    1                             <NA>                                  <NA>
    2                             <NA>                                  <NA>
    3                             <NA>                                  <NA>
    4                             <NA>                                  <NA>
    5                             <NA>                                  <NA>
    6                             <NA>                                  <NA>
      winlog.event_data.VsmPolicy winlog.event_data.LastBootGood
    1                        <NA>                           <NA>
    2                        <NA>                           <NA>
    3                        <NA>                           <NA>
    4                        <NA>                           <NA>
    5                        <NA>                           <NA>
    6                        <NA>                           <NA>
      winlog.event_data.LastBootId winlog.event_data.BootStatusPolicy
    1                         <NA>                               <NA>
    2                         <NA>                               <NA>
    3                         <NA>                               <NA>
    4                         <NA>                               <NA>
    5                         <NA>                               <NA>
    6                         <NA>                               <NA>
      winlog.event_data.LastShutdownGood winlog.event_data.BootMenuPolicy
    1                               <NA>                             <NA>
    2                               <NA>                             <NA>
    3                               <NA>                             <NA>
    4                               <NA>                             <NA>
    5                               <NA>                             <NA>
    6                               <NA>                             <NA>
      winlog.event_data.BootType winlog.event_data.LoadOptions
    1                       <NA>                          <NA>
    2                       <NA>                          <NA>
    3                       <NA>                          <NA>
    4                       <NA>                          <NA>
    5                       <NA>                          <NA>
    6                       <NA>                          <NA>
      winlog.event_data.EntryCount winlog.event_data.BitlockerUserInputTime
    1                         <NA>                                     <NA>
    2                         <NA>                                     <NA>
    3                         <NA>                                     <NA>
    4                         <NA>                                     <NA>
    5                         <NA>                                     <NA>
    6                         <NA>                                     <NA>
      winlog.event_data.CountNew winlog.event_data.CountOld
    1                       <NA>                       <NA>
    2                       <NA>                       <NA>
    3                       <NA>                       <NA>
    4                       <NA>                       <NA>
    5                       <NA>                       <NA>
    6                       <NA>                       <NA>
      winlog.event_data.UpdateReason winlog.event_data.EnabledNew
    1                           <NA>                         <NA>
    2                           <NA>                         <NA>
    3                           <NA>                         <NA>
    4                           <NA>                         <NA>
    5                           <NA>                         <NA>
    6                           <NA>                         <NA>
      winlog.event_data.DeviceNameLength winlog.event_data.DeviceName
    1                               <NA>                         <NA>
    2                               <NA>                         <NA>
    3                               <NA>                         <NA>
    4                               <NA>                         <NA>
    5                               <NA>                         <NA>
    6                               <NA>                         <NA>
      winlog.event_data.DeviceTime winlog.event_data.FinalStatus
    1                         <NA>                          <NA>
    2                         <NA>                          <NA>
    3                         <NA>                          <NA>
    4                         <NA>                          <NA>
    5                         <NA>                          <NA>
    6                         <NA>                          <NA>
      winlog.event_data.DeviceVersionMajor winlog.event_data.DeviceVersionMinor
    1                                 <NA>                                 <NA>
    2                                 <NA>                                 <NA>
    3                                 <NA>                                 <NA>
    4                                 <NA>                                 <NA>
    5                                 <NA>                                 <NA>
    6                                 <NA>                                 <NA>
      winlog.event_data.DriveName winlog.event_data.CorruptionActionState
    1                        <NA>                                    <NA>
    2                        <NA>                                    <NA>
    3                        <NA>                                    <NA>
    4                        <NA>                                    <NA>
    5                        <NA>                                    <NA>
    6                        <NA>                                    <NA>
      winlog.event_data.State winlog.event_data.MinimumPerformancePercent
    1                    <NA>                                        <NA>
    2                    <NA>                                        <NA>
    3                    <NA>                                        <NA>
    4                    <NA>                                        <NA>
    5                    <NA>                                        <NA>
    6                    <NA>                                        <NA>
      winlog.event_data.MaximumPerformancePercent
    1                                        <NA>
    2                                        <NA>
    3                                        <NA>
    4                                        <NA>
    5                                        <NA>
    6                                        <NA>
      winlog.event_data.PerformanceImplementation winlog.event_data.Group
    1                                        <NA>                    <NA>
    2                                        <NA>                    <NA>
    3                                        <NA>                    <NA>
    4                                        <NA>                    <NA>
    5                                        <NA>                    <NA>
    6                                        <NA>                    <NA>
      winlog.event_data.Number winlog.event_data.IdleStateCount
    1                     <NA>                             <NA>
    2                     <NA>                             <NA>
    3                     <NA>                             <NA>
    4                     <NA>                             <NA>
    5                     <NA>                             <NA>
    6                     <NA>                             <NA>
      winlog.event_data.IdleImplementation winlog.event_data.NominalFrequency
    1                                 <NA>                               <NA>
    2                                 <NA>                               <NA>
    3                                 <NA>                               <NA>
    4                                 <NA>                               <NA>
    5                                 <NA>                               <NA>
    6                                 <NA>                               <NA>
      winlog.event_data.MinimumThrottlePercent winlog.event_data.Config
    1                                     <NA>                     <NA>
    2                                     <NA>                     <NA>
    3                                     <NA>                     <NA>
    4                                     <NA>                     <NA>
    5                                     <NA>                     <NA>
    6                                     <NA>                     <NA>
      winlog.event_data.IsTestConfig winlog.event_data.Default SD String:
    1                           <NA>                                 <NA>
    2                           <NA>                                 <NA>
    3                           <NA>                                 <NA>
    4                           <NA>                                 <NA>
    5                           <NA>                                 <NA>
    6                           <NA>                                 <NA>
      winlog.event_data.AdapterSuffixName winlog.event_data.DnsServerList
    1                                <NA>                            <NA>
    2                                <NA>                            <NA>
    3                                <NA>                            <NA>
    4                                <NA>                            <NA>
    5                                <NA>                            <NA>
    6                                <NA>                            <NA>
      winlog.event_data.Sent UpdateServer winlog.event_data.Ipaddress
    1                                <NA>                        <NA>
    2                                <NA>                        <NA>
    3                                <NA>                        <NA>
    4                                <NA>                        <NA>
    5                                <NA>                        <NA>
    6                                <NA>                        <NA>
      winlog.event_data.ErrorCode winlog.event_data.AdapterName
    1                        <NA>                          <NA>
    2                        <NA>                          <NA>
    3                        <NA>                          <NA>
    4                        <NA>                          <NA>
    5                        <NA>                          <NA>
    6                        <NA>                          <NA>
      winlog.event_data.HostName winlog.event_data.TimeSource winlog.event_id
    1                       <NA>                         <NA>            4703
    2                       <NA>                         <NA>            4673
    3                       <NA>                         <NA>              10
    4                       <NA>                         <NA>              10
    5                       <NA>                         <NA>              10
    6                       <NA>                         <NA>              10
                     winlog.provider_name  winlog.api winlog.record_id
    1 Microsoft-Windows-Security-Auditing wineventlog            50588
    2 Microsoft-Windows-Security-Auditing wineventlog           104875
    3            Microsoft-Windows-Sysmon wineventlog           226649
    4            Microsoft-Windows-Sysmon wineventlog           153525
    5            Microsoft-Windows-Sysmon wineventlog           163488
    6            Microsoft-Windows-Sysmon wineventlog           153526
      winlog.computer_name winlog.process.pid winlog.process.id winlog.keywords
    1      HR001.shire.com                  4              4108   Audit Success
    2     HFDC01.shire.com                  4              5144   Audit Failure
    3      IT001.shire.com               3220              4972            NULL
    4      HR001.shire.com               3136              4672            NULL
    5    ACCT001.shire.com               3184              5028            NULL
    6      HR001.shire.com               3136              4672            NULL
                        winlog.provider_guid                       winlog.channel
    1 {54849625-5478-4994-a5ba-3e3b0328c30d}                             security
    2 {54849625-5478-4994-a5ba-3e3b0328c30d}                             Security
    3 {5770385f-c22a-43e0-bf4c-06f5698ffbd9} Microsoft-Windows-Sysmon/Operational
    4 {5770385f-c22a-43e0-bf4c-06f5698ffbd9} Microsoft-Windows-Sysmon/Operational
    5 {5770385f-c22a-43e0-bf4c-06f5698ffbd9} Microsoft-Windows-Sysmon/Operational
    6 {5770385f-c22a-43e0-bf4c-06f5698ffbd9} Microsoft-Windows-Sysmon/Operational
                                 winlog.task winlog.opcode winlog.version
    1            Token Right Adjusted Events          Info             NA
    2                Sensitive Privilege Use          Info             NA
    3 Process accessed (rule: ProcessAccess)          Info              3
    4 Process accessed (rule: ProcessAccess)          Info              3
    5 Process accessed (rule: ProcessAccess)          Info              3
    6 Process accessed (rule: ProcessAccess)          Info              3
      winlog.user.domain winlog.user.type winlog.user.identifier winlog.user.name
    1               <NA>             <NA>                   <NA>             <NA>
    2               <NA>             <NA>                   <NA>             <NA>
    3       NT AUTHORITY             User               S-1-5-18           SYSTEM
    4       NT AUTHORITY             User               S-1-5-18           SYSTEM
    5       NT AUTHORITY             User               S-1-5-18           SYSTEM
    6       NT AUTHORITY             User               S-1-5-18           SYSTEM
      winlog.activity_id winlog.user_data.ProcessID winlog.user_data.ProviderPath
    1               <NA>                       <NA>                          <NA>
    2               <NA>                       <NA>                          <NA>
    3               <NA>                       <NA>                          <NA>
    4               <NA>                       <NA>                          <NA>
    5               <NA>                       <NA>                          <NA>
    6               <NA>                       <NA>                          <NA>
      winlog.user_data.xml_name winlog.user_data.ProviderName winlog.user_data.Code
    1                      <NA>                          <NA>                  <NA>
    2                      <NA>                          <NA>                  <NA>
    3                      <NA>                          <NA>                  <NA>
    4                      <NA>                          <NA>                  <NA>
    5                      <NA>                          <NA>                  <NA>
    6                      <NA>                          <NA>                  <NA>
      winlog.user_data.HostProcess winlog.user_data.Id
    1                         <NA>                <NA>
    2                         <NA>                <NA>
    3                         <NA>                <NA>
    4                         <NA>                <NA>
    5                         <NA>                <NA>
    6                         <NA>                <NA>
      winlog.user_data.PossibleCause winlog.user_data.ClientProcessId
    1                           <NA>                             <NA>
    2                           <NA>                             <NA>
    3                           <NA>                             <NA>
    4                           <NA>                             <NA>
    5                           <NA>                             <NA>
    6                           <NA>                             <NA>
      winlog.user_data.Component winlog.user_data.Operation winlog.user_data.User
    1                       <NA>                       <NA>                  <NA>
    2                       <NA>                       <NA>                  <NA>
    3                       <NA>                       <NA>                  <NA>
    4                       <NA>                       <NA>                  <NA>
    5                       <NA>                       <NA>                  <NA>
    6                       <NA>                       <NA>                  <NA>
      winlog.user_data.ResultCode winlog.user_data.ClientMachine
    1                        <NA>                           <NA>
    2                        <NA>                           <NA>
    3                        <NA>                           <NA>
    4                        <NA>                           <NA>
    5                        <NA>                           <NA>
    6                        <NA>                           <NA>
      winlog.user_data.SessionID winlog.user_data.ESS winlog.user_data.CONSUMER
    1                       <NA>                 <NA>                      <NA>
    2                       <NA>                 <NA>                      <NA>
    3                       <NA>                 <NA>                      <NA>
    4                       <NA>                 <NA>                      <NA>
    5                       <NA>                 <NA>                      <NA>
    6                       <NA>                 <NA>                      <NA>
      winlog.user_data.Namespace winlog.user_data.Processid
    1                       <NA>                       <NA>
    2                       <NA>                       <NA>
    3                       <NA>                       <NA>
    4                       <NA>                       <NA>
    5                       <NA>                       <NA>
    6                       <NA>                       <NA>
      winlog.user_data.NamespaceName winlog.user_data.Query
    1                           <NA>                   <NA>
    2                           <NA>                   <NA>
    3                           <NA>                   <NA>
    4                           <NA>                   <NA>
    5                           <NA>                   <NA>
    6                           <NA>                   <NA>
      winlog.user_data.Provider winlog.user_data.queryid winlog.user_data.Session
    1                      <NA>                     <NA>                     <NA>
    2                      <NA>                     <NA>                     <NA>
    3                      <NA>                     <NA>                     <NA>
    4                      <NA>                     <NA>                     <NA>
    5                      <NA>                     <NA>                     <NA>
    6                      <NA>                     <NA>                     <NA>
      winlog.user_data.Reason winlog.user_data.Address winlog.user_data.messageName
    1                    <NA>                     <NA>                         <NA>
    2                    <NA>                     <NA>                         <NA>
    3                    <NA>                     <NA>                         <NA>
    4                    <NA>                     <NA>                         <NA>
    5                    <NA>                     <NA>                         <NA>
    6                    <NA>                     <NA>                         <NA>
      winlog.user_data.ListenerName winlog.user_data.Class
    1                          <NA>                   <NA>
    2                          <NA>                   <NA>
    3                          <NA>                   <NA>
    4                          <NA>                   <NA>
    5                          <NA>                   <NA>
    6                          <NA>                   <NA>
      winlog.user_data.listenerName version      name
    1                          <NA>   1.1.0 WECServer
    2                          <NA>   1.1.0 WECServer
    3                          <NA>   1.1.0 WECServer
    4                          <NA>   1.1.0 WECServer
    5                          <NA>   1.1.0 WECServer
    6                          <NA>   1.1.0 WECServer
                        agent.ephemeral_id agent.hostname
    1 b372be1f-ba0a-4d7e-b4df-79eac86e1fde      WECServer
    2 b372be1f-ba0a-4d7e-b4df-79eac86e1fde      WECServer
    3 b372be1f-ba0a-4d7e-b4df-79eac86e1fde      WECServer
    4 b372be1f-ba0a-4d7e-b4df-79eac86e1fde      WECServer
    5 b372be1f-ba0a-4d7e-b4df-79eac86e1fde      WECServer
    6 b372be1f-ba0a-4d7e-b4df-79eac86e1fde      WECServer
                                  agent.id agent.version agent.type
    1 d347d9a4-bff4-476c-b5a4-d51119f78250         7.4.0 winlogbeat
    2 d347d9a4-bff4-476c-b5a4-d51119f78250         7.4.0 winlogbeat
    3 d347d9a4-bff4-476c-b5a4-d51119f78250         7.4.0 winlogbeat
    4 d347d9a4-bff4-476c-b5a4-d51119f78250         7.4.0 winlogbeat
    5 d347d9a4-bff4-476c-b5a4-d51119f78250         7.4.0 winlogbeat
    6 d347d9a4-bff4-476c-b5a4-d51119f78250         7.4.0 winlogbeat

2\. Привести датасеты в вид “аккуратных данных”, преобразовать типы
столбцов в соответствии с типом данных

3\. Просмотрите общую структуру данных с помощью функции glimpse()

#### Анализ

##### очки доступа

1\. Раскройте датафрейм избавившись от вложенных датафреймов. Для
обнаружения таких можно использовать функцию dplyr::glimpse() , а для
раскрытия вложенности – tidyr::unnest() . Обратите внимание, что при
раскрытии теряются внешние названия колонок – это можно предотвратить
если использовать параметр tidyr::unnest(…, names_sep = ) .

2\. Минимизируйте количество колонок в датафрейме – уберите колоки с
единственным значением параметра.

3\. Какое количество хостов представлено в данном датасете?

4\. Подготовьте датафрейм с расшифровкой Windows Event_ID, приведите
типы данных к типу их значений.

5\. Есть ли в логе события с высоким и средним уровнем значимости?
Сколько их?

## Оценка результатов

## Вывод
