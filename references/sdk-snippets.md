# Omni Video — ready-to-paste SDK snippets

All snippets read the key from `OMNIVIDEO_API_KEY`. Issue one at <https://omnivideo.net/>.

## Python — `omnivideo-sdk`

```bash
pip install omnivideo-sdk
```

```python
from omnivideo_sdk import OmniVideo

client = OmniVideo()
task = client.run(
    model_id="seedance-2",
    prompt="a serene zen garden at sunrise, ultra detailed",
    aspect_ratio="16:9",
)
print(task.output_url)
```

## Node / TypeScript — `omnivideo-sdk`

```bash
npm i omnivideo-sdk
```

```ts
import { OmniVideo } from "omnivideo-sdk";

const client = new OmniVideo();
const task = await client.run({
  model_id: "seedance-2",
  prompt: "a serene zen garden at sunrise, ultra detailed",
  aspect_ratio: "16:9",
});
console.log(task.video_url ?? task.image_url);
```

## Ruby — `omnivideo-sdk`

```bash
gem install omnivideo-sdk
```

```ruby
require "omnivideo_sdk"

client = OmnivideoSdk::Client.new
task = client.run(model_id: "seedance-2",
                  prompt: "a serene zen garden at sunrise",
                  aspect_ratio: "16:9")
puts task.output_url
```

## Go — `omnivideo-sdk-go`

```bash
go get github.com/omnivideo/omnivideo-sdk-go
```

```go
client, _ := omnivideo.NewClient("")
task, _ := client.Run(ctx, omnivideo.CreateTaskInput{
    ModelID: "seedance-2", Prompt: "a serene zen garden",
}, omnivideo.RunOptions{})
fmt.Println(task.OutputURL())
```

## PHP — `omnivideo/omnivideo-sdk`

```bash
composer require omnivideo/omnivideo-sdk
```

```php
$client = new \OmniVideo\Client();
$task = $client->run([
    'model_id' => 'seedance-2',
    'prompt'   => 'a serene zen garden',
    'aspect_ratio' => '16:9',
]);
echo $task->outputUrl();
```

## Raw curl (any environment)

```bash
# 1) submit
curl -s https://omnivideo.net/api/v1/tasks/create \
  -H "Authorization: Bearer $OMNIVIDEO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model_id":"gpt-image-2","prompt":"a serene zen garden","aspect_ratio":"1:1"}'

# 2) poll
curl -s https://omnivideo.net/api/v1/tasks/abcdef123456 \
  -H "Authorization: Bearer $OMNIVIDEO_API_KEY"
```
