
### 從 iOS App 呼叫自訂 API
若要從 iOS 用戶端呼叫此自訂 API，請使用 `MSClient invokeAPI` 方法。此方法有兩個版本，一個適用於 JSON 格式的要求，另一個適用於任何資料類型：

    /// Invokes a user-defined API of the Mobile Service.  The HTTP request and
    /// response content will be treated as JSON.
    -(void)invokeAPI:(NSString *)APIName
                body:(id)body
          HTTPMethod:(NSString *)method
          parameters:(NSDictionary *)parameters
             headers:(NSDictionary *)headers
          completion:(MSAPIBlock)completion;

    /// Invokes a user-defined API of the Mobile Service.  The HTTP request and
    /// response content can be of any media type.
    -(void)invokeAPI:(NSString *)APIName
                data:(NSData *)data
          HTTPMethod:(NSString *)method
          parameters:(NSDictionary *)parameters
             headers:(NSDictionary *)headers
          completion:(MSAPIDataBlock)completion;


例如，若要將 JSON 要求傳送至名為 "sendEmail" 的自訂 API，請傳遞 `NSDictionary` 要求參數：

    NSDictionary *emailHeader = @{ @"to": @"email.com", @"subject" : @"value" };

    [self.client invokeAPI:@"sendEmail"
         body:emailBody
         HTTPMethod:@"POST"
         parameters:emailHeader
         headers:nil
         completion:completion ];

如果您需要傳回資料，您可以使用類似這樣的方法：

    [self.client invokeAPI:apiName
                 body:yourBody
           HTTPMethod:httpMethod
           parameters:parameters
              headers:headers
           completion:  ^(NSData *result,
                          NSHTTPURLResponse *response,
                          NSError *error){
               // error is nil if no error occured
               if(error) { 
                   NSLog(@"ERROR %@", error);
               } else {

        // do something with the result
               }


           }];



<!---HONumber=AcomDC_0204_2016-->