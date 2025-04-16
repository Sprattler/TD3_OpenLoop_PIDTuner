# TD3_OpenLoop_PIDTuner https://colab.research.google.com/drive/1QB8XaZrV4zqNe3CXjNnNrL9vJrng7IkW?usp=sharing
# Pendulum PID Tuning with TD3

Dieses Projekt nutzt den Twin Delayed Deep Deterministic Policy Gradient (TD3) Algorithmus, um die Parameter eines PID-Reglers für eine simulierte Pendelumgebung zu bestimmen. Ziel ist es, Optimale PID (Kp, Ki, Kd) Faktoren zu ermitteln.
## Übersicht

- **Umgebung**: Angepasste Gym-Umgebung, die ein Pendel mit  Schwerkraft, Länge, Masse und Dämpfung simuliert. Der Zustand umfasst den Winkel-Fehler, das Integral des Fehlers und die Ableitung des Fehlers.
- **PID-Regler**: Als lineare Richtlinie im Actor-Netzwerk implementiert, das das Drehmoment als `u = Kp*Fehler + Ki*Integral + Kd*Ableitung` berechnet, wobei \[Kp, Ki, Kd\] lernbare Gewichte sind.
- **TD3 Algorithmus**: Ein Off-Policy-Reinforcement-Learning-Algorithmus, der die PID-Gewinne anpasst, um die kumulative Belohnung pro Episode zu maximieren. 
- **Belohnungsfunktion**: Definiert als `-|3*Fehler + Integral|`, um sowohl den Fehler als auch das Integral des Fehlers zu minimieren. -> Diese Funktion ist den Anforderrungen Anzupassen

## Hauptkomponenten

- **Actor-Netzwerk**: Eine lineare Schicht mit nicht-negativen Gewichten, die die PID-Gewinne repräsentiert. ->  x1 * Kp + x2 * Ki + x3 * Kd + 0
- **Critic-Netzwerk**: Ein Zwillingsnetzwerk, das Q-Werte für Zustands-Aktions-Paare schätzt.
- **Replay Buffer**: Speichert Erfahrungen für das Training der Netzwerke.
- **Trainingsloop**: Führt Episoden mit den aktuellen PID-Gewinnen durch, sammelt Erfahrungen und aktualisiert die Netzwerke mit TD3.
- **Epsiode**: Das PendelEnv loggt die daten selbst und gibt sie nach dem vollständigen Zyklus in die restliche Trainingsloop.

## Konfiguration

- **Episoden**: 650
- **Maximale Schritte pro Episode**: 1500 (1,5 Sekunden mit dt=0.001)
- **ACTOR_LR**:  - 5e-3
- **CRITIC_LR**: - 5e-3
- **DISCOUNT**: 0.97
- **TAU**: 0.005
- **POLICY_NOISE**: 0.2
- **Rauschbegrenzung**: 0.5
- **POLICY_FREQ = 2**: Alle 2 Iterationen
- **EXPLORATION_NOISE**: 0
- **Batch-Größe**: 256
- **Replay Buffer Größe**: 1.000.000
- **Initiale PID-Parameter**: Kp=50, Ki=1.0, Kd=1.0  !!TODO!!


## Ergebnisse

Der Trainingsprozess protokolliert Episodenbelohnungen, Endwinkel und PID-Parameter in Weights & Biases. Die besten PID-Parameter basierend auf der höchsten Episodenbelohnung werden am Ende ausgegeben.
