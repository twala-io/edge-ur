# Uploader Job to Estuary

- Accepts concurrent uploads (small to large)
- Stores the CID and content on the local blockstore using whypfs
- Save the data on local sqlite DB
- Process each files and call estuary add-ipfs endpoint to make deals for the CID
- uses estuary api (`content/add-ipfs`) endpoint to pin files on estuary

![image](https://user-images.githubusercontent.com/4479171/211354164-2df9b2be-ff77-4749-871b-3a5932e0b857.png)

# Build
## `go build`
```
go build -tags netgo -ldflags '-s -w' -o edge-ur
```

# Running 
## Create the `.env` file
```
DB_NAME=edge-ur
UPLOAD_ENDPOINT=https://api.estuary.tech/content/add-ipfs
```

## Running the daemon
```
./edge-ur daemon
```

# Gateway
This node comes with it's own gateway to serve directories and files.

View the gateway using:
- https://localhost:1313
- https://localhost:1313/dashboard
- https://localhost:1313/gw/ipfs/:cid

# Pin and make a storage deal for your file(s) on Estuary
```
curl --location --request POST 'http://localhost:1313/api/v1/content/add' \
--header 'Authorization: Bearer [ESTUARY_API_KEY]' \
--form 'data=@"/path/to/file"'
```

# Pin make a storage deal for your cid(s) on Estuary
```
curl --location --request POST 'http://localhost:1313/api/v1/content/cid/bafybeihxodfkobqiovfgui6ipealoabr2u3bhor765z47wxdthrgn7rvyq' \
--header 'Authorization: Bearer [ESTUARY_API_KEY]'
```

## Status check
This will return the status of the file(s) or cid(s) on edge-ur. It'll also return the estuary content_id.
```
curl --location --request GET 'http://localhost:1313/api/v1/status/5' \
--header 'Authorization: Bearer [ESTUARY_API_KEY]'
```
