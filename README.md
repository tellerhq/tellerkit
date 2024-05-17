# Teller Connect for iOS and macOS

Welcome to the native bindings for Teller Connect.

## Example usage

### SwiftUI

A view modifier `tellerConnect` is provided that will modally present Teller Connect over the modified view using sheet presentation. 

#### Notes

- On iPadOS a custom sheet presentation is used to ensure the sheet is the correct size for Teller Connect
- On macOS Teller Connect is opened inside of its own NSWindow of the correct size for Teller Connect

#### Example

```swift
import SwiftUI
import TellerKit

struct TellerView: View {
    @State var isPresented : Bool = false
    
    var body: some View {
    	Button(action: {
            isPresented = true
        }) {
            Text("Connect Account")
                .padding()
                .buttonStyle(.bordered)
                .font(.system(size: 18.0))
        }
        .padding(.all, 5)
        .tellerConnect(isPresented: $isPresented, config: Teller.Config(
            appId: "APP_ID",
            environment: .sandbox,
            selectAccount: .single,
            products: [.transactions, .verify]
        ) { [self] reg in
            switch reg {
            case .exit:
                break
            case .enrollment(let auth):
                print("auth \(auth)")
            default:
                break
            }
        })
    }
}
```

### UIKit and AppKit

You can create an instance of `Teller.ViewController` using `Teller.ViewController(configuration: Teller.Config)` that can be presented in the usual ways. 
