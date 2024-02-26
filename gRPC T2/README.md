//install


go get -u google.golang.org/grpc

go get -u google.golang.org/protobuf


// generate go code from protobuf

protoc --go_out=proto --go_opt=paths=source_relative --go-grpc_out=proto --go-grpc_opt=paths=source_relative proto/*.proto