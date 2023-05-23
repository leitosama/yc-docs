## Настройте разрешения сетевого трафика {#network-settings}

[Группы безопасности](../../vpc/concepts/security-groups.md) работают как виртуальный брандмауэр для входящего и исходящего трафика. См. подробнее о [группе безопасности по умолчанию](../../vpc/concepts/security-groups.md#default-security-group).

Если группы безопасности недоступны в вашем облаке, будет разрешен любой трафик.

1. Для работы OpenVPN Access Server [добавьте](../../vpc/operations/security-group-add-rule.md) следующие правила в группу безопасности:

   | Направление</br>трафика | Описание | Порт | Протокол | Тип</br>источника | Источник/Назначение |
   |---|---|---|---|---|---|
   | Входящий | VPN Server | 443 | TCP | CIDR | 0.0.0.0/0 |
   | Входящий | VPN Server | 1194 | UDP | CIDR | 0.0.0.0/0 |
   | Входящий | Admin Web UI,</br>Client Web UI | 943 | TCP | CIDR | 0.0.0.0/0 |

   Сервер VPN способен перенаправлять трафик с порта `HTTPS`, поэтому при необходимости оставьте открытым единственный порт `TCP 443`. См. также настройки на вкладке **Configuration** → **Network Settings** в административной панели сервера.

1. Если вы настроили собственную группу безопасности, проверьте, что разрешен трафик между сервером VPN и требуемыми ресурсами. Например, они находятся в одной группе и создано [правило Self](../../vpc/concepts/security-groups.md#rules-types) для всей группы.