### 오류처리

- 오류 코드보다 예외 사용
```java
// 오류 코드 반환 코드
public class DeviceController {
...
	public void sendShutDown() {
		DeviceHandle handle = getHandle(DEV1);
        // 디바이스 상태 점검
        if (handle !\ DeviceHandle.INVALID) {
        // 레코드 필드에 디바이스 상태 저장
        retrieveDeviceRecord(handle);
        // 디바이스 일시정지 상태가 아니라면 종료
        if (record.getStatus() != DEVICE_SUSPENDED) {
        	pauseDeviece(handle);
            clearDeviceWorkQueue(handle);
            	closeDevice(handle);
               }else {
                logger.log("Device suspended. Unable to shut Down");
               }
             } else {
               logger.log("Invalid handle for: " + DEV1.toString());
             }
           }
           ...
         }  
         
 // 예외 반환 코드
 public class DeviceController {
 ...
 
 public void sendShutDown() {
  try {
   tryToShutDown();
  } catch (DeviceShutDownError e){
  logger.log(e);
 }
}

private void tryToShutDown() throws Device ShutDownError {
	DeviceHandle handle = getHandle(DEV1);
    DeviceRecord record = retrieveDeviceRecord(handle);
    
    pauseDevice(handle);
    clearDeviceWorkQueue(handle);
    closeDevice(handle);
   }
   
   private DeviceHandle getHandle(DeviceId id) {
   ...
   throw new DeviceShutDownError("Invalid handle for: " + id.toString());
   ...
  }
  
  ...
 }
 ```         

오류 코드 자체를 반환하는 것 보다 예외 처리를 하면 코드가 간결해진다. 또 오류 처리 로직과 디바이스 종료 로직을 분리하여 이해하기 더 쉬운 코드가 완성된다.


- Try-Catch-Finally 문부터 작성하라

테스트 주도적 개발을 위해 저자는 Try-Catch-Finally문을 먼저 작성하여 단위 테스트를 통과시켜야 한다고 주장한다. catch 블록의 프로그램 상태를 일관성 있게 유지하여 작성하면 코드 실행 중 무슨 일이 생기더라도 호출자가 기대하는 상태를 정의하기 쉽다.


- 미확인 예외 사용

대규모 시스템에서는 하위 함수에서 오류를 던지면 상위 함수의 선언부에 throws 절을 추가해야 한다. 이는 최상위 함수로의 의존성 사이에 연쇄적인 수정과 캡슐화를 깨뜨리는 문제를 유발한다.
따라서 일반적인 어플리케이션의 경우 의존성이라는 비용이 이익보다 크다는 점을 생각하며 확인된 예외와 미확인 예외의 사용에 대한 고민이 필요하다.

- 정상 흐름 정의

예외에 의미를 제공하고 호출자를 고려하여 예외 클래스를 정의해야 한다. 오류 처리와 비즈니스 논리를 잘 분리하면 오류 처리를 간결한 알고리즘으로 만들 수 있다.


```java
// 식비를 비용으로 청구했다면 청구한 식비를 총계에 더해고, 청구하지 않았다면 일일 기본 식비를 더하는 로직
try {
 MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
 m_total += expenses.getTotal();
} catch(MealExpensesNotFound e) {
  m_total += getMealPerDiem();
}

// ExpenseReportDAO수정으로 청구한 식비가 없다면 기본 일일 식비를 반환하는 로직
MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expenses.getTotal();

```

- null을 반환, 전달하지 마라

null이라는 값이 반드시 필요한 경우가 아니라면, null을 반환하고 전달하는 것은 책임을 떠넘기는 것과 같다. 대규모 프로그램에서는 어디선가 null을 전달하게 되면 이후 동작에 문제가 생길 수 있고 이를 찾기가 꽤나 까다롭다. 특수 사례 객체나 다른 예외 처리를 통해 이러한 문제를 예방할 수 있다.

```java
// null 반환하는 getEmpoyees
List<Employee> employees = getEmployees();
if (employees != null) {
	for(Employee e : employees) {
    totalPay += e.getPay();
   }
  }
  
// getEmployees를 빈 리스트 반환하도록 변경
List<Employee> employees = getEmployees();
for(Employee e : employees) {
	totalPay += e.getPay();
}

public List<Employee> getEmployees() {
	if (..직원 없을 경우..)
    	return Collections.emptyList();
   }