# Attachment passing through API in Laravel POST, Blob handle using Multipart


// Initialize the Guzzle client with base URI and headers
$client = new Client([
    'base_uri' => 'https://api.verify.test.com',
    'headers' => [
        'Authorization' => 'Bearer <<token>>',
    ],
]);

$promises = []; // Array to hold promises for async requests

$path = "http://27.0.0.1/uploads/documents/mypic.jpg";
$file_name = basename($path); // Get the file name

// Create an async request and add it to the promises array
$promises = $client->postAsync('/api/v2/applications', [
    'multipart' => [
        [
        'name' => 'application[name]',
        'contents' => 'Md Alamin Dawan'
        ],
        [
        'name' => 'application[location]',
        'contents' => 'Bangladesh'
        ],
        [
            'name'     => 'file_index_name[0]',
            'contents' => fopen($path, 'r'),
            'filename' => $file_name,
        ]
        
    ],
]);


if ($promises instanceof \GuzzleHttp\Promise\Promise) {
    $promises = $promises->wait();
}

// Now $response should be an actual Response object
$body = $promises->getBody();
echo $body;exit;
