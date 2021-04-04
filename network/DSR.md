# DSR ( direct server return )
* client -> load balancer -> server -> load balancer ->client 가 아닌
client -> load balancer -> server -> client
* 대용량 파일 업로드의 경우 클라이언트의 요청이 load balancer를 타는것부터 부담이 될수 있다. 
* 대용량 파일 업로드의 경우 로드밸런스를 거치지 않고 다이렉트로 서버에 올리도록 수정하는게 좋다. 
