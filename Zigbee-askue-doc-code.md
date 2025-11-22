---
# Metadata / Конфигурация документа
title: "Работа технологии Zigbee в счетчиках электроэнергии"
version: "1.0.0"
author: "Technical Writer"
date: "2025-11-16"
status: "published"
language: "ru"
standard: "GOST 19.106-78"
tags:
  - zigbee
  - askue
  - wireless
  - metering
  - documentation
audience:
  - engineers
  - specialists
  - technical-writers
  - developers
toc: true
---

# Работа технологии Zigbee в счетчиках электроэнергии

<!-- 
  ДОКУМЕНТ: Техническая документация по протоколу Zigbee
  НАЗНАЧЕНИЕ: Описание принципов работы и применения в системах АСКУЭ
  СООТВЕТСТВИЕ: ГОСТ 19.106-78 «Единая система программной документации»
  STATUS: STABLE (v1.0.0)
-->

## 📋 Содержание

- [1. Введение](#1-введение)
- [2. Общие сведения о технологии Zigbee](#2-общие-сведения-о-технологии-zigbee)
- [3. Технические характеристики](#3-технические-характеристики)
- [4. Архитектура сети Zigbee в системах АСКУЭ](#4-архитектура-сети-zigbee-в-системах-аскуэ)
- [5. Работа Zigbee в счетчиках электроэнергии](#5-работа-zigbee-в-счетчиках-электроэнергии)
- [6. Безопасность и защита данных](#6-безопасность-и-защита-данных)
- [7. Преимущества и недостатки](#7-преимущества-и-недостатки-технологии)
- [8. Применение в различных объектах](#8-применение-в-различных-объектах)
- [9. Требования к эксплуатации](#9-требования-к-эксплуатации)
- [10. Диспетчерское ПО](#10-диспетчерское-программное-обеспечение)
- [Приложения](#приложения)

---

## 1. Введение

### Назначение документа

Настоящий документ описывает принципы работы технологии **Zigbee** в счетчиках электрической энергии в соответствии с требованиями **ГOST 19.106-78**.

### Определения

```yaml
# Основные определения
technology:
  name: "Zigbee"
  type: "Wireless Communication Standard"
  base_protocol: "IEEE 802.15.4"
  purpose: "Low-power wireless networks"
  application: "ASKUE (Automated Metering Systems)"

target_audience:
  - role: "Engineers"
    description: "Project and design engineers"
  - role: "Specialists"
    description: "System exploitation specialists"
  - role: "Technical Writers"
    description: "Documentation specialists"
  - role: "Developers"
    description: "System developers"
```

### Нормативные ссылки

```
├─ IEEE 802.15.4      // Физический уровень (PHY) и MAC
├─ Zigbee Specification // Сетевой уровень и безопасность
└─ GOST 19.106-78      // Требования к документации
```

---

## 2. Общие сведения о технологии Zigbee

### 2.1. Функции в системах АСКУЭ

```yaml
functions:
  - function: "Real-time Data Collection"
    description: "Автоматический сбор показаний со счетчиков"
    priority: "critical"
  
  - function: "Remote Management"
    description: "Дистанционное управление приборами учета"
    priority: "high"
  
  - function: "Self-organizing Network"
    description: "Передача данных по самоорганизующейся сети"
    priority: "high"
  
  - function: "Device Monitoring"
    description: "Мониторинг состояния и выявление вмешательств"
    priority: "medium"
```

### 2.2. Базовые параметры

```ini
# IEEE 802.15.4 / Zigbee Basic Configuration
[Physical_Layer]
standard = "IEEE 802.15.4"
base_frequency_ru = "2400-2483.5 MHz"  # Global band (Russia)
base_frequency_eu = "868 MHz"          # Europe only
base_frequency_us = "915 MHz"          # USA only
max_data_rate = "250 kbps"             # In 2.4 GHz band
modulation = "O-QPSK"
channels_in_ru = 16
channels_in_eu = 1
channels_in_us = 10

[Network_Capabilities]
max_devices_per_network = 65536
topology = "mesh | star | tree"
self_healing = true
dynamic_routing = true
```

---

## 3. Технические характеристики

### 3.1. Частотные параметры

```yaml
frequency_bands:
  global_2400:
    frequency: "2400-2483.5 MHz"
    channels: 16
    data_rate: "250 kbps"
    modulation: "O-QPSK"
    usage_ru: true
    usage_eu: false
    usage_us: false
    
  europe_868:
    frequency: "868 MHz"
    channels: 1
    data_rate: "20 kbps"
    modulation: "BPSK"
    usage_ru: false
    usage_eu: true
    usage_us: false
    
  usa_915:
    frequency: "915 MHz"
    channels: 10
    data_rate: "40 kbps"
    modulation: "BPSK"
    usage_ru: false
    usage_eu: false
    usage_us: true
```

### 3.2. Показатели производительности

```markdown
| Параметр | Значение | Примечание |
|----------|----------|-----------|
| Дальность связи | 10-100 м | В типовых условиях |
| Максимальная дальность | 500 м | При прямой видимости |
| Время жизни батареи | До 10 лет | Для конечных устройств |
| Мощность на открытой местности | 10 мВт | Максимум |
| Мощность в зданиях | 100 мВт | Максимум |
| Скорость передачи (прикладной уровень) | 1-10 кбит/с | Реальная производительность |
```

---

## 4. Архитектура сети Zigbee в системах АСКУЭ

### 4.1. Топология сети

```
Mesh Network Architecture
═════════════════════════

                    ┌─ Coordinator ─┐
                    │   (1 шт.)    │
                    └──────────────┘
                          │
                ┌─────────┼─────────┐
                │         │         │
              Router    Router    Router
              (N шт.)   (N шт.)   (N шт.)
                │         │         │
        ┌───┬───┴──┐  ┌───┴───┐  ┌──┴───┐
        │   │      │  │       │  │      │
       End End    End End     End End   End
      Dev Dev    Dev Dev     Dev Dev   Dev
    (≤65536)
    
Topology Mode: MESH
├─ Self-healing: YES
├─ Dynamic routing: YES
├─ Auto-reconnection: YES
└─ Redundancy: Multiple paths
```

### 4.2. Типы устройств и их роли

```yaml
device_types:
  
  coordinator:
    count_per_network: 1
    role: "Network Creator & Manager"
    power_source: "Mains (220V)"
    capabilities:
      - "Network formation"
      - "Device management"
      - "Central database"
      - "Trust center"
    interfaces:
      - "COM-port"
      - "Ethernet"
      - "GPRS"
    reliability: "critical"
  
  router:
    count_per_network: "unlimited"
    role: "Data Relay & Device Connection"
    power_source: "Mains (9-27V or 220V)"
    capabilities:
      - "Packet retransmission"
      - "End device connection"
      - "Network expansion"
      - "Route optimization"
    coverage_range: "30-50 m (indoors)"
    reliability: "high"
  
  end_device:
    count_per_network: 65536
    role: "Data Source"
    power_source: "Battery or mains"
    capabilities:
      - "Data transmission"
      - "Sleep mode"
      - "Battery-powered operation"
      - "Extended autonomy"
    autonomy: "up to 10 years"
    reliability: "standard"
```

### 4.3. Процесс маршрутизации

```python
# Pseudo-code: Zigbee Mesh Routing Algorithm
class MeshRouter:
    def __init__(self, device_id, network):
        self.device_id = device_id
        self.network = network
        self.routing_table = {}
        self.signal_strength = {}
    
    def discover_routes(self):
        """Discover available routes to destination"""
        candidates = self.network.available_hops
        for hop in candidates:
            signal = self.measure_rssi(hop)
            self.signal_strength[hop] = signal
        return sorted_by_signal_strength(self.signal_strength)
    
    def select_best_route(self, destination):
        """Select optimal route based on:
        - Signal strength (RSSI)
        - Number of hops
        - Link quality
        - Device availability
        """
        routes = self.discover_routes()
        best_route = max(routes, key=lambda r: r.quality_metric)
        return best_route
    
    def transmit_packet(self, data, destination):
        """Transmit data through selected route"""
        route = self.select_best_route(destination)
        # Packet sent via: Dev → Router → Router → Coordinator
        return self.send_via_route(data, route)
```

---

## 5. Работа Zigbee в счетчиках электроэнергии

### 5.1. Интеграция модулей

```yaml
meter_integration:
  
  integrated_modules:
    - model: "Merkury 206 PNOF03"
      zigbee: true
      phases: 1
      protocol: "Zigbee built-in"
    
    - model: "Energomera CE308"
      zigbee: true
      phases: 3
      protocol: "Zigbee built-in"
    
    - model: "TelePosition Project"
      zigbee: true
      phases: "1 or 3"
      protocol: "Zigbee + GPRS gateway"
  
  external_modems:
    - manufacturer: "Energomera"
      interface: "RS-485"
      compatibility: ["CE102", "CE201", "CE301", "CE303"]
      zigbee_enabled: true
```

### 5.2. Процесс передачи данных

```
Data Flow in ASKUE System
═════════════════════════

METER MEASUREMENT
    │
    ├─ Active Energy (kWh)
    ├─ Reactive Energy (kVArh)
    ├─ Power (kW)
    ├─ Voltage (V)
    └─ Current (A)
    │
    ▼
ZIGBEE MODULE ENCRYPTION
    │
    ├─ AES-128 Encryption
    ├─ MIC (Message Integrity Code)
    ├─ Network Key
    └─ Link Key
    │
    ▼
MESH NETWORK TRANSMISSION
    │
    ├─ End Device → Router (1)
    ├─ Router (1) → Router (2)
    ├─ Router (2) → Router (3)
    └─ Last Router → Coordinator
    │
    ▼
COORDINATOR (Gateway)
    │
    ├─ Data Decryption
    ├─ Integrity Check (MIC)
    └─ Forward to Server
    │
    ▼
SERVER / DISPATCHER
    │
    ├─ GPRS Channel
    ├─ Ethernet Connection
    └─ COM-Port Link
    │
    ▼
DISPATCH SOFTWARE (ASKUE)
    │
    ├─ Data Processing
    ├─ Report Generation
    ├─ Billing
    └─ Anomaly Detection
```

---

## 6. Безопасность и защита данных

### 6.1. Криптографические параметры

```yaml
security:
  
  encryption:
    algorithm: "AES-128"
    key_length: 128  # bits
    mode: "CCM*"
    implementation: "IEEE 802.15.4 compliant"
  
  integrity_check:
    method: "MIC (Message Integrity Code)"
    lengths: [0, 32, 64, 128]  # bits
    action_on_fail: "packet_dropped"
  
  authentication:
    type: "Trust Center based"
    roles:
      - "Coordinator acts as Trust Center"
      - "Key distribution"
      - "Device authentication"
  
  key_management:
    network_key:
      purpose: "Protect routing information"
      level: "NWK (Network Layer)"
      distribution: "Coordinator → All devices"
    
    link_key:
      purpose: "Protect application data"
      level: "APL (Application Layer)"
      distribution: "Application specific"
```

### 6.2. Процесс ретрансляции с шифрованием

```
Secure Packet Relay Process
═══════════════════════════

SOURCE DEVICE (End Device)
    │
    └─ Encrypt with Network Key
       │
       ▼
    Encrypted Packet + MIC
       │
    ▼
INTERMEDIATE ROUTER
    │
    ├─ Receive packet
    │
    ├─ Decrypt with Network Key
    │  └─ Verify MIC
    │
    ├─ Check destination:
    │   ├─ If destination reached → Forward
    │   └─ If not → Re-route
    │
    ├─ Encrypt with Network Key (again)
    │
    └─ Forward to next hop
       │
       ▼
    (Repeat for each router)
       │
       ▼
COORDINATOR
    │
    ├─ Final decryption
    ├─ MIC verification
    └─ Application layer processing

Security Level: DOUBLE-ENCRYPTED at each hop
```

---

## 7. Преимущества и недостатки технологии

### 7.1. Comparison Matrix

```yaml
advantages:
  
  cost_efficiency:
    description: "Низкая стоимость развертывания"
    reason: "Отсутствие кабельной инфраструктуры"
    impact: "high"
  
  reliability:
    description: "Высокая надежность сети"
    reason: "Mesh-топология с избыточностью"
    impact: "high"
  
  energy_efficiency:
    description: "Низкое энергопотребление"
    reason: "Оптимизированный протокол"
    autonomy: "до 10 лет на батарейках"
    impact: "high"
  
  scalability:
    description: "Простота расширения"
    reason: "Самоорганизация сети"
    devices_supported: 65536
    impact: "medium"
  
  automation:
    description: "Автоматическая самоорганизация"
    reason: "Встроенные алгоритмы формирования сети"
    manual_intervention: "minimal"
    impact: "high"

disadvantages:
  
  limited_bandwidth:
    description: "Ограниченная пропускная способность"
    max_rate_physical: "250 kbps"
    real_rate_application: "1-10 kbps"
    severity: "medium"
  
  range_limitation:
    description: "Ограниченная дальность связи"
    typical_range: "10-100 m"
    max_range: "500 m"
    solution: "Intermediate repeaters required"
    severity: "medium"
  
  frequency_congestion:
    description: "Перегруженность спектра в городах"
    affected_areas: ["urban", "dense_buildings"]
    solution: "Careful network planning"
    severity: "high"
  
  compatibility_issues:
    description: "Проблемы совместимости разных стеков"
    cause: "Proprietary implementations"
    workaround: "Use same manufacturer equipment"
    severity: "high"
```

---

## 8. Применение в различных объектах

### 8.1. Use Cases по типам объектов

```yaml
use_cases:
  
  residential:
    type: "Multi-apartment buildings"
    placement: "Apartment entrance, common areas"
    topology: "Mesh with gateway in basement"
    gateway_connection: "GPRS to energy company"
    benefits:
      - "No cable installation"
      - "Easy meter replacement"
      - "Real-time readings"
    challenges:
      - "Multi-floor signal attenuation"
      - "Signal propagation through concrete"
  
  suburban:
    type: "Cottage villages, SNT"
    placement: "Utility poles, building entrances"
    topology: "Mesh with routers for coverage"
    gateway_connection: "GPRS or Ethernet"
    benefits:
      - "Low deployment cost"
      - "Optimal frequency spectrum utilization"
      - "Minimal electromagnetic interference"
    challenges:
      - "Distance between meters"
      - "Weather impact on signal"
  
  industrial:
    type: "Distributed industrial sites"
    placement: "Switchboards, distribution points"
    topology: "Mesh with central coordinator"
    gateway_connection: "SCADA integration"
    benefits:
      - "Real-time energy consumption monitoring"
      - "Load management capabilities"
      - "Equipment status tracking"
    challenges:
      - "High electromagnetic interference"
      - "Complex integration requirements"
```

---

## 9. Требования к эксплуатации

### 9.1. Environmental Specifications

```ini
[Environmental_Conditions]
operating_temperature_min = -40 °C
operating_temperature_max = +70 °C
storage_temperature_min = -50 °C
storage_temperature_max = +85 °C
humidity = "5-95% (non-condensing)"
altitude = "0-2000 m"

[Mechanical_Requirements]
vibration_resistance = "standard"
shock_resistance = "standard"
enclosure_rating = "IP54 minimum"
```

### 9.2. Installation Checklist

```markdown
## Инструкция по установке

### Координатор (Coordinator)
- [ ] Установка в диспетчерском пункте
- [ ] Питание: мains (220V) или ИБП
- [ ] Подключение: COM-port, Ethernet или GPRS
- [ ] Конфигурация AT-команд
- [ ] Задание PAN ID
- [ ] Выбор частотного канала
- [ ] Тестирование соединения

### Маршрутизаторы (Routers)
- [ ] DIN-рейка установка
- [ ] Питание: 9-27V или 220V адаптер
- [ ] Размещение с учетом зоны покрытия (30-50м)
- [ ] Программирование роли устройства
- [ ] Параметры безопасности (сетевой ключ)
- [ ] Тестирование связи с координатором

### Счетчики (End Devices)
- [ ] Стандартная установка на DIN-рейку
- [ ] Включение питания
- [ ] Автоматическое присоединение к сети
- [ ] Проверка показаний в диспетчерском ПО
- [ ] Мониторинг состояния устройства
```

### 9.3. Configuration Parameters

```yaml
configuration:
  
  coordinator:
    pan_id: "0x1234"                 # Уникальный ID сети
    channel: "auto"                  # Автоматический выбор
    security_level: 5                # 0-7, рекомендуется 5+
    tx_power: "0 dBm"               # Мощность передачи
    
  router:
    pan_id: "inherit"                # От координатора
    joining_enabled: true            # Разрешить присоединение
    security_level: 5
    tx_power: "3 dBm"
    
  meter_polling:
    protocol: "Modbus RTU"           # Protocol selection
    address: "0x01-0xFF"             # Meter address
    baudrate: "9600 bps"             # RS-485 speed
    poll_interval: "300 sec"         # 5 minutes
```

---

## 10. Диспетчерское программное обеспечение

### 10.1. Software Architecture

```
ASKUE Software Stack
═══════════════════

┌─────────────────────────────────────┐
│   Dispatcher Application (UI/UX)    │
│   ├─ Dashboard                      │
│   ├─ Reports & Billing              │
│   ├─ Anomaly Detection              │
│   └─ Configuration Panel            │
└────────────┬────────────────────────┘
             │
┌────────────▼────────────────────────┐
│   ASKUE Core Module                 │
│   ├─ Data Collection Engine         │
│   ├─ Processing Rules               │
│   ├─ Validation Logic               │
│   └─ Archive Management             │
└────────────┬────────────────────────┘
             │
┌────────────▼────────────────────────┐
│   Zigbee Service Layer              │
│   ├─ PROMODEM ZigBeeService         │
│   ├─ COM-port / TCP Handler         │
│   ├─ Protocol Conversion            │
│   │   └─ Modbus TCP ↔ Modbus RTU    │
│   └─ Device Management              │
└────────────┬────────────────────────┘
             │
┌────────────▼────────────────────────┐
│   Coordinator Gateway               │
│   ├─ Network Management             │
│   ├─ Encryption/Decryption         │
│   ├─ Device Registry               │
│   └─ Data Relay                     │
└────────────┬────────────────────────┘
             │
       Zigbee Network
     (Mesh Topology)
```

### 10.2. Software Requirements

```yaml
software_requirements:
  
  protocol_support:
    - "Modbus RTU"
    - "Modbus TCP"
    - "Merkury"
    - "Energomera"
    - "SET"
  
  communication_interfaces:
    - "COM-port (RS-232 / RS-485)"
    - "Ethernet (TCP/IP)"
    - "GPRS (optional)"
  
  database_support:
    - "PostgreSQL"
    - "MySQL"
    - "MS SQL Server"
  
  operating_systems:
    - "Windows Server 2012+"
    - "Linux (Ubuntu 18.04+)"
    - "Docker containers"
```

---

## Приложения

### Приложение А. Терминология

```yaml
terminology:
  
  askue:
    full_name: "Автоматизированная Система Коммерческого Учета Электроэнергии"
    english: "Automated Metering System"
    type: "Acronym"
    context: "Power metering systems"
  
  mesh_topology:
    definition: "Ячеистая децентрализованная топология с множественными избыточными соединениями"
    characteristics: [self_healing, dynamic_routing, redundancy]
  
  uspd:
    full_name: "Устройство Сбора и Передачи Данных"
    function: "Data collection and transmission device"
    role: "Gateway between meters and dispatcher"
  
  mic:
    full_name: "Message Integrity Code"
    purpose: "Verify data integrity during transmission"
    length: "0, 32, 64, or 128 bits"
```

### Приложение Б. Сокращения

```yaml
abbreviations:
  
  network_level:
    WPAN: "Wireless Personal Area Network"
    IEEE: "Institute of Electrical and Electronics Engineers"
    PHY: "Physical Layer"
    MAC: "Medium Access Control"
    NWK: "Network Layer"
    APL: "Application Layer"
  
  protocols:
    TCP: "Transmission Control Protocol"
    IP: "Internet Protocol"
    GPRS: "General Packet Radio Service"
    GSM: "Global System for Mobile Communications"
    AES: "Advanced Encryption Standard"
    CCM: "Counter with CBC-MAC"
  
  devices:
    ООД: "Оконечное Оборудование Данных"
    ИБП: "Источник Бесперебойного Питания"
    УК: "Управляющая Компания"
    СНТ: "Садовое Некоммерческое Товарищество"
    ИЖС: "Индивидуальное Жилищное Строительство"
    ДНП: "Дачное Некоммерческое Партнерство"
  
  measurements:
    kWh: "Киловатт-часы"
    kW: "Киловатты"
    V: "Вольты"
    A: "Амперы"
    MHz: "Мегагерцы"
    kbps: "Килобиты в секунду"
```

### Приложение В. Справочные данные

```yaml
reference_data:
  
  frequency_specifications:
    - band: "868 MHz (Europe)"
      channels: 1
      data_rate: "20 kbps"
      modulation: "BPSK"
    
    - band: "915 MHz (USA)"
      channels: 10
      data_rate: "40 kbps"
      modulation: "BPSK"
    
    - band: "2400-2483.5 MHz (Global/Russia)"
      channels: 16
      data_rate: "250 kbps"
      modulation: "O-QPSK"
  
  device_specifications:
    - type: "Coordinator"
      count: 1
      power: "Mains"
      range: "N/A (coordinator)"
    
    - type: "Router"
      count: "Unlimited"
      power: "Mains (9-27V / 220V)"
      range: "30-50 m (indoors)"
    
    - type: "End Device"
      count: 65536
      power: "Battery / Mains"
      range: "10-100 m (typical)"
  
  security_matrix:
    - parameter: "Encryption Algorithm"
      value: "AES-128"
      standard: "IEEE 802.15.4"
    
    - parameter: "Key Length"
      value: "128 bits"
      standard: "FIPS 197"
    
    - parameter: "Integrity Check (MIC)"
      value: "0/32/64/128 bits"
      standard: "CCM*"
```

---

## Дополнительные ресурсы

### Документы и стандарты

```
References
══════════

[1] IEEE 802.15.4-2020
    Standard for Low-Rate Wireless Personal Area Networks (LR-WPANs)
    
[2] Zigbee Specification v3.0
    Connectivity Standards Alliance
    
[3] GOST 19.106-78
    Единая система программной документации
    
[4] ГОСТ Р 53380.2-2013
    Системы распределения электроэнергии. Часть 2
```

### Related Documentation

```
├─ ASKUE Implementation Guide
├─ Zigbee Network Planning Manual
├─ Meter Integration Handbook
├─ Security Best Practices
├─ Troubleshooting Guide
└─ FAQ & Common Issues
```

---

## История версий

```yaml
versions:
  
  1.0.0:
    date: "2025-11-16"
    status: "published"
    changes: "Initial release"
    sections: 12
    pages: "11 (PDF equivalent)"
  
  # Версия 1.1.0 (планируется)
  # - Добавление примеров кода Python
  # - Расширение раздела интеграции с SCADA
  # - Включение примеров конфигурации
```

---

**Примечание**: Этот документ структурирован как "Documentation as Code" для удобства версионирования, интеграции в Git и автоматизации обработки.

```yaml
# Конец документа / End of Document
metadata:
  format: "Markdown (RFC 822 compatible)"
  encoding: "UTF-8"
  line_ending: "LF"
  last_modified: "2025-11-16T20:30:00Z"
  status: "stable"
  license: "CC-BY-4.0"
```
