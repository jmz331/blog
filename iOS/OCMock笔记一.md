###利用OCMock来方便单体测试的时候做数据注入

```
- (void)testNormal{
    // This is an example of a functional test case.
    /**
     *  单例类方法绑定
     */
    id mockSettingsToTest = OCMPartialMock([Settings sharedInstance]);
    OCMStub([mockSettingsToTest email]).andReturn(@"multiuser@multi.isr.co.jp");
    
    Settings *settings = [Settings sharedInstance];
    
    XCTAssertTrue([settings.email isEqualToString:@"multiuser@multi.isr.co.jp"], @"email : %@", settings.email);
    
    [mockSettingsToTest stopMocking];
    
    NSLog(@"KEYCHAIN_EMAIL.1:%@",[ISRKeyChainUtil valueForKey:KEYCHAIN_EMAIL]);
    
    XCTAssertTrue([settings.email isEqualToString:@"HelloWorld"], @"email : %@", settings.email);
    
    /**
     *  静态类方法绑定
     */
    id mockISRKeyChainUtilToTest = OCMClassMock([ISRKeyChainUtil class]);
    OCMStub(ClassMethod([mockISRKeyChainUtilToTest valueForKey:@"email"])).andReturn(@"multiuser@multi.isr.co.jp");
    
    XCTAssertTrue([[ISRKeyChainUtil valueForKey:KEYCHAIN_EMAIL] isEqualToString:@"multiuser@multi.isr.co.jp"], @"email : %@", [ISRKeyChainUtil valueForKey:KEYCHAIN_EMAIL]);
    
    [mockISRKeyChainUtilToTest stopMocking];
    
    XCTAssertTrue([[ISRKeyChainUtil valueForKey:KEYCHAIN_EMAIL] isEqualToString:@"HelloWorld"], @"email : %@", [ISRKeyChainUtil valueForKey:KEYCHAIN_EMAIL]);
}
```
