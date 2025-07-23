Установка контекста
kubectl create namespace app-pods
kubectl config set-context --current --namespace=app-pods

Запуск подов

kubectl run front-end-app --image=nginx --labels role=front-end --expose --port 80
kubectl run back-end-api-app --image=nginx --labels role=back-end-api --expose --port 80
kubectl run admin-front-end-app --image=nginx --labels role=admin-front-end --expose --port 80
kubectl run admin-back-end-api-app --image=nginx --labels role=admin-back-end-api --expose --port 80


Применение политик (отдельная политика ingress для каждого из подов)
kubectl apply -f non-admin-api-allow.yaml
    

Проверка доступности
kubectl exec -it front-end-app -- curl http://back-end-api-app # Wlcome nginx page
kubectl exec -it back-end-api-app -- curl http://front-end-app # Wlcome nginx page
kubectl exec -it admin-front-end-app -- curl http://admin-back-end-api-app # Wlcome nginx page
kubectl exec -it admin-back-end-api-app -- curl http://admin-front-end-app # Welcome nginx page

Проверка недоступности
kubectl exec -it front-end-app -- curl --connect-timeout 2 http://admin-back-end-api-app # таймаут
kubectl exec -it back-end-api-app -- curl --connect-timeout 2 http://admin-front-end-app # таймаут
kubectl exec -it admin-front-end-app -- curl --connect-timeout 2  http://back-end-api-app # таймаут
kubectl exec -it admin-back-end-api-app -- curl --connect-timeout 2 http://front-end-app # таймаут
kubectl run test-111 --rm -i -t --image=alpine -- curl --connect-timeout 2  http://front-end-app # таймаут
    

    