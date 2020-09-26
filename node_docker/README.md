# Optional Lab (building docker image from node.js source)

```bash
docker build -t node-js-demo:1.0 .
docker run --rm -d -p 8000:8080 node-js-demo:1.0
curl http://localhost:8000
```
