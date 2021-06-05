## gitlab cicd with docker

* runner -> registry 등록 gitlab은 registry랑만 소통한다
gitlab이 필요한 이미지를 registry를 fetch해간다.
* docker는 runner를 통해서 gitlab과 소통한다.
* local과 소통하고 싶을때는 shell executer