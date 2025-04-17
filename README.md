// 定義腳位
const int switchPin1 = 2;  // 第一個開關
const int switchPin2 = 3;  // 第二個開關 
const int switchPin3 = 4;  // 第三個開關
const int switchPin4 = 5;  // 第四個開關
const int ledPin1 = 9;    // LED 1 (PWM腳位)
const int ledPin2 = 10;   // LED 2 (PWM腳位)
const int ledPin3 = 11;   // LED 3 (PWM腳位)
const int trigPin = 12;   // 超音波感測器 Trig
const int echoPin = 13;   // 超音波感測器 Echo

// 全域變數
int brightness = 0;    // LED亮度
int fadeAmount = 5;    // 呼吸燈漸變量
unsigned long previousMillis = 0;  // 計時器
const long interval = 100;        // 閃爍間隔

void setup() {
  // 設定輸入輸出腳位
  pinMode(switchPin1, INPUT_PULLUP);
  pinMode(switchPin2, INPUT_PULLUP);
  pinMode(switchPin3, INPUT_PULLUP);
  pinMode(switchPin4, INPUT_PULLUP);
  pinMode(ledPin1, OUTPUT);
  pinMode(ledPin2, OUTPUT);
  pinMode(ledPin3, OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void loop() {
  // 讀取超音波感測器距離
  long duration, distance;
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  // 如果感測到物體(距離小於20cm)，三燈同時閃爍
  if (distance < 20) {
    unsigned long currentMillis = millis();
    if (currentMillis - previousMillis >= interval) {
      previousMillis = currentMillis;
      static boolean ledState = LOW;
      ledState = !ledState;
      digitalWrite(ledPin1, ledState);
      digitalWrite(ledPin2, ledState);
      digitalWrite(ledPin3, ledState);
    }
    return;  // 跳過其他功能
  }

  // 讀取開關狀態
  if (digitalRead(switchPin1) == LOW) {  // 呼吸燈模式
    analogWrite(ledPin1, brightness);
    delay(30);
    analogWrite(ledPin2, brightness);
    delay(30);
    analogWrite(ledPin3, brightness);
    
    brightness = brightness + fadeAmount;
    if (brightness <= 0 || brightness >= 255) {
      fadeAmount = -fadeAmount;
    }
  }
  else if (digitalRead(switchPin2) == LOW) {  // 順序閃爍模式
    digitalWrite(ledPin1, HIGH);
    delay(200);
    digitalWrite(ledPin1, LOW);
    digitalWrite(ledPin2, HIGH);
    delay(200);
    digitalWrite(ledPin2, LOW);
    digitalWrite(ledPin3, HIGH);
    delay(200);
    digitalWrite(ledPin3, LOW);
  }
  else if (digitalRead(switchPin3) == LOW) {  // 恆亮模式
    digitalWrite(ledPin1, HIGH);
    digitalWrite(ledPin2, HIGH);
    digitalWrite(ledPin3, HIGH);
  }
  else if (digitalRead(switchPin4) == LOW) {  // 關閉所有燈
    digitalWrite(ledPin1, LOW);
    digitalWrite(ledPin2, LOW);
    digitalWrite(ledPin3, LOW);
  }
}
