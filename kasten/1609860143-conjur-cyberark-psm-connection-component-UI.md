# 1609860143 conjur-cyberark-psm-connection-component-UI

```xml
<ConnectionComponent EnableWindowScrollbar="Yes" FullScreen="No" Height="768" Id="PSM-DAP CLI" Type="CyberArk.PasswordVault.TransparentConnection.PSM.PSMConnectionComponent, CyberArk.PasswordVault.TransparentConnection.PSM" Width="1024">
  <ComponentParameters/>
  <UserParameters>
    <Parameter Name="AllowMappingLocalDrives" Required="Yes" Type="CyberArk.TransparentConnection.BooleanUserParameter, CyberArk.PasswordVault.TransparentConnection" Value="No" Visible="No"/>
    <Parameter EnforceInDualControlRequest="Yes" Name="ConnectAs" Type="CyberArk.PasswordVault.TransparentConnection.PSM.OracleConnectAsUserParameter, CyberArk.PasswordVault.TransparentConnection.PSM" Value="Normal"/>
  </UserParameters>
  <TargetSettings ClientApp="powershell -NoExit -Command &quot;echo yes | conjur init --url https://{&#x200B;&#x200B;Address}&#x200B;&#x200B; -a cybr --force=yes; conjur authn login -u {&#x200B;&#x200B;Username}&#x200B;&#x200B; -p {&#x200B;&#x200B;Password}&#x200B;&#x200B;&quot;" ClientDispatcher="NA" ClientInvokeType="CommandLine" Protocol="Rest">
    <ClientSpecific/>
    <LockAppWindow Enable="No" MainWindowClass="ConsoleWindowClass" SearchWindowWaitTimeout="30" Timeout="1000"/>
    <Capabilities/>
  </TargetSettings>
</ConnectionComponent>
```


## Links
