# lab7_docker: Konfiguracja i wykorzystanie builder-ów buildx. Budowa obrazów dla wielu architektur sprzętowych. Zarządzanie danymi cache z procesu budowania

# [Link do DockerHub](https://hub.docker.com/r/miloszpiechota/repository)




## 1. Sprawdzenie dostępnych builderów:

`docker buildx ls`

![Zrzut ekranu 2024-05-09 205430](https://github.com/miloszpiechota/lab7_docker/assets/161620373/3194bfeb-b56e-4c95-82b0-03037f96286f)


## 2.Tworzenie własnego buildera - lab7builder z wykorzystaniem sterownika docker-container:

`docker buildx create --name lab7builder --driver docker-container --bootstrap`

![Zrzut ekranu 2024-05-09 205913](https://github.com/miloszpiechota/lab7_docker/assets/161620373/6e412386-b959-44c8-a4c3-dd4d0b71a2f5)


## 3.Ustawienie builder-a lab7builder jako domyślnego i jego aktywacja

`docker buildx use lab7builder`
`docker buildx ls`


## 4. Budowanie obrazu dla ARM64 i x86_64/amd64 oraz nadanie mu tagu "spg51/labx:7l"

`docker buildx build -f DockerfileA -t miloszpiechota/repozytorium:l7 --push .`

![image](https://github.com/miloszpiechota/lab7_docker/assets/161620373/9e0d36ad-2d8d-409a-b051-c259661f69c4)


## Wyświetlenie szczegółowych informacji o obrazie Docker, takich jak metadane, warstwy obrazu, konfiguracja, etykiety i inne informacje.

`docker buildx imagetools inspect spg51/labx:l7`

![image](https://github.com/miloszpiechota/lab7_docker/assets/161620373/1ee595d2-1112-4c64-bf09-73ff3c47e3ab)


## To polecenie buduje obraz Docker na podstawie DockerfileA, wykorzystując cache z rejestru Docker, eksportując go również do rejestru i przesyłając go do rejestru po zakończeniu budowy.

`docker buildx build -f DockerfileA --platform linux/arm64,linux/amd64 -t miloszpiechota/repository:7l --cache-to=type=registry,ref=miloszpiechota/repository:cache,mode=max --cache-from=type=registry,ref=miloszpiechota/repository:cache --push .`

![image](https://github.com/miloszpiechota/lab7_docker/assets/161620373/2b5c9c30-7a1a-45fc-84b0-9b9e54e34dfa)
![image](https://github.com/miloszpiechota/lab7_docker/assets/161620373/0d8cb188-e4bd-4e5d-acec-19709f5060fa)


![image](https://github.com/miloszpiechota/lab7_docker/assets/161620373/7aef5ccb-7af0-403d-af46-9f6651784af4)

Ta linia wskazuje, że próba zaimportowania manifestu cache z rejestru zakończyła się błędem. Jest to normalne zachowanie, gdyż w przypadku pierwszej budowy obrazu nie ma jeszcze danych cache w rejestrze, więc Docker próbuje pobrać je, ale nie znajduje ich.


![image](https://github.com/miloszpiechota/lab7_docker/assets/161620373/7ed01b99-0206-40e1-9ae1-12550749e2d3)



