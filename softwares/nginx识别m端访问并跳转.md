```shell

server {
    error_page 400 401 402 403 405 408 410 412 413 414 415 500 501 502 503 506 @error1;
    error_page 404 @error2;

    location @error1 {
        if ($http_user_agent ~* "(android|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows (ce|phone)|xda|xiino"){
            rewrite ^/(.*)$ http://m.xxx.com/m/coming.html break;

        }
        rewrite ^/(.*)$  http://m.xxx.com/p/coming.html break;                    
    }

    location @error2{
        if ($http_user_agent ~* "(android|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows (ce|phone)|xda|xiino"){
            rewrite ^/(.*)$ http://m.xxx.com/m/404.html break;

        }
        rewrite ^/(.*)$ http://m.xxx.com/p/404.html break;                
    } 
}
```