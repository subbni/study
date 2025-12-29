# ConcurrentHashMap
- 읽기 작업에는 여러 쓰레드가 동시 접근 가능
- 쓰기 작업에는 특정 버킷에 대한 Lock을 사용
- 다른 버킷이라면 동시에 쓰기 작업 가능하여 경쟁이 발생하지 않기 때문에 락 획득을 위한 대기 시간을 줄일 수 있음

<img width="679" height="626" alt="스크린샷 2025-12-29 15 47 21" src="https://github.com/user-attachments/assets/b504eeef-b087-4996-bbe8-ed4a87d2a9b7" />
