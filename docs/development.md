# Настройка окружения

## USB в Windows

Для работы с отладчиком из Docker через WSL2 используется `usbipd-win`.

1. Установите `usbipd-win`:

   ```powershell
   winget install usbipd
   ```

2. Подключите отладчик и выполните `usbipd list`. Найдите Mikron-Link / FTDI — обычно `Dual RS232-HS`, VID:PID `0403:6010`.
3. В PowerShell от имени администратора выполните привязку:

   ```powershell
   usbipd bind --busid <BUSID> --force
   ```

4. Пробросьте устройство в WSL:

   ```powershell
   usbipd attach --auto-attach --wsl --busid <BUSID>
   ```

5. Откройте проект в VS Code и выполните **Dev Containers: Reopen in Container**.

После переподключения USB повторите проброс. Для USB-UART аналогично подключите устройство `USB-SERIAL CH340`.

В [devcontainer.json](../.devcontainer/devcontainer.json) заданы `/dev/ttyUSB0` и `/dev/ttyUSB1`. Для сборки без USB удалите соответствующие параметры `--device` из `runArgs`.

## UART

В терминале контейнера:

```sh
make monitor SERIAL_PORT=/dev/ttyUSB0
```

Скорость — 115200 бод. Выход из `picocom`: `Ctrl+A`, затем `Ctrl+X`.

## Отладка

Выполните `make flash`, затем запустите конфигурацию **Cortex Debug** из [.vscode/launch.json](../.vscode/launch.json). Она использует OpenOCD и `build/firmware/mik32-rps.elf`.

Загрузка программы выполняется через `make flash` и `mik32-uploader`; команда GDB `load` в этом окружении не используется.
