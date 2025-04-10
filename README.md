// 定義腳位
const int trigPin = 9;    // 超聲波發射腳
const int echoPin = 10;   // 超聲波接收腳
const int ledPin = 13;    // LED腳位

void setup() {
  // 初始化串口通訊
  Serial.begin(9600);
  
  // 設置腳位模式
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(ledPin, OUTPUT);
}

void loop() {
  // 發送超聲波
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // 接收超聲波並計算距離
  long duration = pulseIn(echoPin, HIGH);
  float distance = duration * 0.034 / 2;  // 轉換為厘米
  
  // 輸出距離到串口監視器
  Serial.print("距離: ");
  Serial.print(distance);
  Serial.println(" 厘米");
  
  // 如果距離小於20厘米，點亮LED
  if (distance < 20) {
    digitalWrite(ledPin, HIGH);
  } else {
    digitalWrite(ledPin, LOW);
  }
  
  delay(500);  // 延遲500毫秒
}
