# go-zero


goctl api go --api ./my.api -dir . --style go_zero

goctl api validate --api ./api/my.api

protoc --go_out=. --go_opt=paths=source_relative  --go-grpc_out=. --go-grpc_opt=paths=source_relative guide.proto

protoc --go_out=. --go_opt=paths=source_relative --go-grpc_out=. --go-grpc_opt=paths=source_relative my.proto

python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. my.proto 

python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. ./xxpath/protos/my.proto 

