import SwiftUI

final class SessionModel: {
    @Published var fontScale: Double {
        didSet { save() }
    }
    @Published var Money: Double {
        didSet { save() }
    }
    @Published var RevClic: Double {
        didSet { save() }
    }
    @Published var IntervalTime: Double {
        didSet { save() }
    }
    @Published var totalMoneyEarned: Double {
        didSet { save() }
    }
    
    init() {
        self.fontScale = UserDefaults.standard.double(forKey: "fontScale") != 0 ? UserDefaults.standard.double(forKey: "fontScale") : 200
        self.Money = UserDefaults.standard.double(forKey: "Money")
        self.RevClic = UserDefaults.standard.double(forKey: "RevClic") != 0 ? UserDefaults.standard.double(forKey: "RevClic") : 0.01
        self.IntervalTime = UserDefaults.standard.double(forKey: "IntervalTime") != 0 ? UserDefaults.standard.double(forKey: "IntervalTime") : 1
        self.totalMoneyEarned = UserDefaults.standard.double(forKey: "totalMoneyEarned")
    }

    
    private func save() {
        UserDefaults.standard.set(fontScale, forKey: "fontScale")
        UserDefaults.standard.set(Money, forKey: "Money")
        UserDefaults.standard.set(RevClic, forKey: "RevClic")
        UserDefaults.standard.set(IntervalTime, forKey: "IntervalTime")
        UserDefaults.standard.set(totalMoneyEarned, forKey: "totalMoneyEarned")
    }
    
    private func load() {
        fontScale = UserDefaults.standard.double(forKey: "fontScale") == 0 ? 200 : UserDefaults.standard.double(forKey: "fontScale")
        Money = UserDefaults.standard.double(forKey: "Money")
        RevClic = UserDefaults.standard.double(forKey: "RevClic") == 0 ? 0.01 : UserDefaults.standard.double(forKey: "RevClic")
        IntervalTime = UserDefaults.standard.double(forKey: "IntervalTime") == 1 ? 1 : UserDefaults.standard.double(forKey: "IntervalTime")
        totalMoneyEarned = UserDefaults.standard.double(forKey: "totalMoneyEarned")
    }
}


struct ContentView: View{
    @StateObject var session = SessionModel()
    @State private var isBreathing: Bool = false
    @State private var tapPulse: Bool = false
    @State private var leftPanelHeight: CGFloat = 0
    @State private var comboDecayTimer: Timer? = nil
    @State private var feverTimer: Timer? = nil
    @State private var achievementBanner: String? = nil
    @State private var showAchievement: Bool = false
    @State private var IsOn: Bool = false
    @State private var NumberOfServers: Double = 1
    var body: some View{
            ZStack{
                Rectangle()
                    .foregroundStyle(Color.black)
                    .blur(radius: 3.0)
                VStack{
                    
                    Text("Youtube clicker")
                        .font(.system(size: 50, weight: .black, design: .monospaced))
                        .opacity(1.0)
                        .foregroundStyle(Color.green)
                        .shadow(color: .green, radius: 10)
                    RoundedRectangle(cornerRadius: 10)
                        .foregroundStyle(Color.green)
                        .frame(height: 2)
                    HStack{
                        Spacer()
                        Text("by Proxima Nova")
                            .contextMenu(ContextMenu(menuItems: {
                                Text("Abonnez Vous")
                            }))
                    }
                    HStack{
                        ZStack{
                            Rectangle()
                                .foregroundStyle(Color.black)
                                .blur(radius: 3.0)
                            VStack{
                                Text("Votre chaîne")
                                    .font(.system(size: 25, weight: .medium, design: .monospaced))
                                    .foregroundStyle(Color.green)
                                    .padding(10)
                                    .shadow(color: .white, radius: 10)
                                    .textFieldStyle(RoundedBorderTextFieldStyle())
                                    .contextMenu {
                                        Button("Vérification anti bot") {
                                            print("Vérificarion effectuée. Google manager est sur le coup.")
                                        }
                                    }
                                RoundedRectangle(cornerRadius: 10)
                                    .frame(height: 2)
                                    .foregroundStyle(Color.green)
                                HStack{
                                    Text(String(format: "Monnaie: %.2f", session.Money))
                                        .font(.system(size: 25, weight: .bold, design: .monospaced))
                                        .foregroundStyle(Color.red)
                                        .shadow(color: .red, radius: 10)
                                    Image("Monetisation")
                                        .resizable()
                                        .scaledToFit()
                                        .frame(width: 40, height: 40)
                                }
                                RoundedRectangle(cornerRadius: 10)                
                                    .frame(height: 2)
                                    .foregroundStyle(Color.red)
                                ZStack {
                                    Circle()
                                        .fill(Color.red.opacity(0.15))
                                        .frame(width: session.fontScale * 1.5, height: session.fontScale * 1.4)
                                        .scaleEffect(isBreathing ? 1.06 : 0.94)
                                        .animation(.easeInOut(duration: 1.2).repeatForever(autoreverses: true), value: isBreathing)
                                        .blur(radius: 2)
                                    Circle()
                                        .stroke(Color.red.opacity(0.6), lineWidth: 4)
                                        .frame(width: session.fontScale * 2.0, height: session.fontScale * 2.0)
                                        .scaleEffect(tapPulse ? 1.6 : 0.2)
                                        .opacity(tapPulse ? 0.0 : 1.0)
                                        .animation(.easeOut(duration: 0.4), value: tapPulse)
                                    Button(action: {
                                        session.Money += session.RevClic
                                        CodeClickerSession.TotalClicks += 1  
                                        session.totalMoneyEarned += session.RevClic
                                    }){
                                        Text("💻")
                                            .font(.system(size: session.fontScale, weight: .bold, design: .monospaced))
                                            .foregroundStyle(Color.green)
                                            .shadow(color: .green, radius: 10)
                                    }
                                    .buttonStyle(.plain)
                                    .font(.system(size: session.fontScale))
                                    .onAppear(perform: {
                                        startTimer()
                                    })
                                }
                            }
                        }
                        RoundedRectangle(cornerRadius: 10)
                            .frame(width: 2, height: 600)
                            .foregroundStyle(Color.green)
                        ZStack{
                            Rectangle()
                                .foregroundStyle(Color.black)
                                .blur(radius: 3.0)
                            VStack{
                                AdPlacementView{}
                                Button(action: {IsOn = true}, label: {
                                    Image("Videos") 
                                        .resizable()
                                        .scaledToFit()
                                })
                                .sheet(isPresented: $IsOn) { 
                                    ScrollView{
                                        Text("Videos")
                                        
                                    }
                                }
                            }
                        }
                        RoundedRectangle(cornerRadius: 10)
                            .frame(width: 2, height: 600)
                            .foregroundStyle(Color.green)
                        ZStack{
                            Color.green
                            
                            List{
                                ClickerView(clickers: CodeLine)
                                ClickerView(clickers: TextEditor)
                                ClickerView(clickers: VersionControl)
                                ClickerView(clickers: CodeEditor)
                                ClickerView(clickers: CodeSnippetLibrary)
                                ClickerView(clickers: PC)
                                ClickerView(clickers: OnlineCourse)
                                ClickerView(clickers: Debugger)
                                ClickerView(clickers: CodeReviewService)
                                ClickerView(clickers: SoftwareLicense)
                                ClickerView(clickers: WebHosting)
                                ClickerView(clickers: CloudStorage)
                                ClickerView(clickers: GraphicCard)
                                ClickerView(clickers: Database)
                                ClickerView(clickers: APIIntegration)
                                ClickerView(clickers: MobileApp)
                                ClickerView(clickers: APIAccess)
                                ClickerView(clickers: GameDevelopmentKit)
                                ClickerView(clickers: Coder)
                                ClickerView(clickers: MachineLearning)
                                ClickerView(clickers: TrainingData)
                                ClickerView(clickers: CloudComputing)
                                ClickerView(clickers: BlockchainTechnology)
                                ClickerView(clickers: VirtualMachine)
                                ClickerView(clickers: PCTower)
                                ClickerView(clickers: DevOpsTool)
                                ClickerView(clickers: SwiftConsole)
                                ClickerView(clickers: EthicHacker)
                                ClickerView(clickers: IA)
                                ClickerView(clickers: QuantumComputer)
                                ClickerView(clickers: SuperCalculator)
                                ClickerView(clickers: InfinityStone)
                                AdPlacementView {
                                }
                                Button("Réinitialiser") {
                                    session.RevClic = 0.01
                                    session.Money = 0
                                    session.fontScale = 200
                                    CodeLine.Quantity = 0
                                    CodeLine.DefaultPrice = 1
                                    PC.Quantity = 0
                                    PC.DefaultPrice = 15
                                    GraphicCard.Quantity = 0
                                    GraphicCard.DefaultPrice = 50
                                    Coder.Quantity = 0
                                    Coder.DefaultPrice = 1000
                                    PCTower.Quantity = 0
                                    PCTower.DefaultPrice = 5000
                                    EthicHacker.Quantity = 0
                                    EthicHacker.DefaultPrice = 10000
                                    IA.Quantity = 0
                                    IA.DefaultPrice = 500000
                                    QuantumComputer.Quantity = 0
                                    QuantumComputer.DefaultPrice = 1000000
                                    session.totalMoneyEarned = 0
                                    print("Réinitialisation terminée.")
                                }
                                .buttonStyle(BorderedProminentButtonStyle())
                            }
                            .listStyle(PlainListStyle())
                            .environmentObject(session)
                            .onAppear(perform: {
                                startTimer()
                                startMove()
                            })
                        }
                    }
                }
        }
    }
    private func startTimer() {
        Timer.scheduledTimer(withTimeInterval: session.IntervalTime, repeats: true) { _ in
            DispatchQueue.main.async {
                session.Money += session.RevClic
                session.totalMoneyEarned += session.RevClic
            }
        }
    }
    private func startMove() {
        Timer.scheduledTimer(withTimeInterval: session.IntervalTime, repeats: true) { _ in
            if session.fontScale == 180 {
                session.fontScale = 160
            } else if session.fontScale == 160 {
                session.fontScale = 180
            }
        }
    }
}
