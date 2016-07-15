FROM golang:1.5.4

# Install revel
RUN go get github.com/revel/revel && \
	go get github.com/revel/cmd/revel

EXPOSE 9000

CMD tail -F /var/log/kern.log