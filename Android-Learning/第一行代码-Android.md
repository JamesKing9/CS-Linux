# <第一行代码-Android>

### 12.3.2 模仿微信摇一摇
```
用加速度传感器来模仿一下微信的摇一摇功能。
其实主体逻辑也非常简单，只需要检测手机在 X 轴、Y 轴和 Z 轴上的加速度，当达到了预定值的时候就可以认为用户摇动了手机，从而触发摇一摇的逻辑。那么现在问题在于，这个预定值应该设定为多少呢？由于重力加速度的存在，即使手机在静止的情况下，某一个轴上的加速度也有可能达到9.8m/s2，因此这个预定值必定是要大于 9.8m/s2的，这里我们就设定为 15m/s2吧。
```
新建一个 AccelerometerSensorTest 项目，然后修改 MainActivity 中的代码，如下所示：
```java
public class MainActivity extends Activity {
  private SensorManager sensorManager;
  
  @Override
  protected void onCreate(Bundle saveInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    sensorManager = (SensorManager)getSystemService(Context.SENSOR_SERVICE);
    Sensor sensor = sensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER);
    sensorManager.registerListener(listener, sensor, SensorManager.SENSOR_DELAY_NORMAL);
  }
  
  @Override
  protected void onDestroy() {
    super.onDestroy();
    if(sensorManager != null) {
      sensorManager.unregisterListener(listener);
    }
  }
  
  private SensorEventListener listener = new SensorEvnetListener() {
    @Override
    public void onSensorChanged(SensorEvent event) {
      float xValue = Math.abs(event.values[0]);
      float yValue = Math.abs(event.values[1]);
      float zValue = Math.abs(event.values[2]);
      if (xValue > 15 || yValue > 15 || zValue >15) {
        Toast.makeText(MainActivity.this, "摇一摇", Toast.LENGTH_SHORT).show();
      }
    }
    
    @Override
    public void onAccuracyChanged(Sensor sensor, int accuracy)
  }
}
```

