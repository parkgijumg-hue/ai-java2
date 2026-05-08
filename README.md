**README.md** 내용입니다. 그대로 복사해서 `README.md` 파일로 저장하세요.

```markdown
# Simple Calculator (Java Swing)

고급 UI와 세련된 디자인을 적용한 **자바 스윙(SimpleCalculator)** 계산기입니다.

## 📌 프로젝트 소개

이 프로젝트는 Java Swing을 이용해 만든 **데스크톱 계산기**입니다.  
사칙연산(+, −, ×, ÷)을 지원하며, 예쁜 색상 테마, 이모지 버튼, 이미지 로딩, 에러 처리, 결과 다이얼로그 등 현대적인 UI 요소를 포함하고 있습니다.

### 주요 기능
- 실수(double) 계산 지원
- 사칙연산 버튼 (색상 구분)
- 0으로 나누기 에러 처리
- 입력값 검증 (NumberFormatException 처리)
- 계산 결과 모달 다이얼로그
- 리소스 이미지 로딩 (`calculator_icon.png`)
- 깔끔한 레이아웃과 테마 색상 적용

---

## 📁 프로젝트 구조

```bash
SimpleCalculator/
├── src/
│   └── SimpleCalculator.java          # 메인 클래스 (모든 코드 포함)
├── resources/
│   └── calculator_icon.png            # 계산기 아이콘 (선택)
├── README.md
└── .gitignore (선택)
```

### 추천 구조 (Maven/Gradle 프로젝트로 확장 시)

```bash
SimpleCalculator/
├── src/
│   └── main/
│       ├── java/
│       │   └── com/
│       │       └── example/
│       │           └── SimpleCalculator.java
│       └── resources/
│           └── calculator_icon.png
├── pom.xml (Maven) 또는 build.gradle
└── README.md
```

---

## 🚀 실행 방법

1. **Java 8 이상**이 설치되어 있어야 합니다.
2. 프로젝트를 IDE (IntelliJ IDEA, Eclipse, VS Code 등)로 열기
3. `SimpleCalculator.java` 파일을 컴파일 및 실행
4. **resources 폴더**에 `calculator_icon.png` 파일을 넣으면 상단에 아이콘이 표시됩니다.

### 명령어로 실행 (터미널)

```bash
javac SimpleCalculator.java
java SimpleCalculator
```

---

## 📖 코드 전체

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.net.URL;

public class SimpleCalculator extends JFrame implements ActionListener {

    private JTextField num1Field;
    private JTextField num2Field;
    private JLabel resultLabel;
    private JLabel imageLabel;
    private JButton addButton, subtractButton, multiplyButton, divideButton;

    // 색상 정의
    private static final Color FRAME_BACKGROUND = new Color(240, 240, 240);
    private static final Color PANEL_BACKGROUND_INPUT = new Color(255, 255, 255);
    private static final Color PANEL_BACKGROUND_BUTTONS = new Color(220, 220, 220);
    private static final Color BUTTON_FOREGROUND = Color.WHITE;

    private static final Color ADD_COLOR = new Color(50, 205, 50);      // LimeGreen
    private static final Color SUBTRACT_COLOR = new Color(255, 69, 0);  // OrangeRed
    private static final Color MULTIPLY_COLOR = new Color(70, 130, 180); // SteelBlue
    private static final Color DIVIDE_COLOR = new Color(153, 50, 204);  // DarkOrchid

    public SimpleCalculator() {
        setTitle("Simple Calculator");
        setSize(400, 380);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);
        getContentPane().setBackground(FRAME_BACKGROUND);

        // Top Panel
        JPanel topPanel = new JPanel(new BorderLayout(10, 10));
        topPanel.setBackground(FRAME_BACKGROUND);

        JLabel titleLabel = new JLabel("Advanced Calculator", SwingConstants.CENTER);
        titleLabel.setFont(new Font("SansSerif", Font.BOLD, 20));
        titleLabel.setForeground(new Color(50, 50, 50));
        topPanel.add(titleLabel, BorderLayout.NORTH);

        imageLabel = new JLabel();
        imageLabel.setHorizontalAlignment(SwingConstants.CENTER);
        ImageIcon icon = loadImageIcon("calculator_icon.png");
        if (icon != null) {
            imageLabel.setIcon(icon);
        } else {
            imageLabel.setText("Image Placeholder");
            imageLabel.setForeground(Color.GRAY);
        }
        topPanel.add(imageLabel, BorderLayout.CENTER);

        // Input Panel
        JPanel inputPanel = new JPanel(new GridLayout(2, 2, 10, 10));
        inputPanel.setBackground(PANEL_BACKGROUND_INPUT);
        inputPanel.setBorder(BorderFactory.createCompoundBorder(
            BorderFactory.createEmptyBorder(10, 10, 10, 10),
            BorderFactory.createTitledBorder(BorderFactory.createLineBorder(Color.LIGHT_GRAY), "Inputs")
        ));

        num1Field = new JTextField();
        num1Field.setFont(new Font("SansSerif", Font.PLAIN, 16));
        num1Field.setHorizontalAlignment(JTextField.RIGHT);
        inputPanel.add(new JLabel("First Number:"));
        inputPanel.add(num1Field);

        num2Field = new JTextField();
        num2Field.setFont(new Font("SansSerif", Font.PLAIN, 16));
        num2Field.setHorizontalAlignment(JTextField.RIGHT);
        inputPanel.add(new JLabel("Second Number:"));
        inputPanel.add(num2Field);

        // Button Panel
        JPanel buttonPanel = new JPanel(new FlowLayout(FlowLayout.CENTER, 15, 15));
        buttonPanel.setBackground(PANEL_BACKGROUND_BUTTONS);
        buttonPanel.setBorder(BorderFactory.createCompoundBorder(
            BorderFactory.createEmptyBorder(10, 10, 10, 10),
            BorderFactory.createTitledBorder(BorderFactory.createLineBorder(Color.LIGHT_GRAY), "Operations")
        ));

        addButton = createOperatorButton("\u2795", ADD_COLOR);
        subtractButton = createOperatorButton("-", SUBTRACT_COLOR);
        multiplyButton = createOperatorButton("\u00D7", MULTIPLY_COLOR);
        divideButton = createOperatorButton("/", DIVIDE_COLOR);

        buttonPanel.add(addButton);
        buttonPanel.add(subtractButton);
        buttonPanel.add(multiplyButton);
        buttonPanel.add(divideButton);

        // Result Label
        resultLabel = new JLabel(" ");
        resultLabel.setHorizontalAlignment(SwingConstants.CENTER);
        resultLabel.setFont(new Font("SansSerif", Font.PLAIN, 14));
        resultLabel.setForeground(Color.RED);

        JPanel bottomPanel = new JPanel(new BorderLayout());
        bottomPanel.setBackground(FRAME_BACKGROUND);
        bottomPanel.add(resultLabel, BorderLayout.CENTER);

        JPanel southRegionPanel = new JPanel(new BorderLayout());
        southRegionPanel.setBackground(FRAME_BACKGROUND);
        southRegionPanel.add(buttonPanel, BorderLayout.CENTER);
        southRegionPanel.add(bottomPanel, BorderLayout.SOUTH);

        // Main Layout
        setLayout(new BorderLayout(10, 10));
        add(topPanel, BorderLayout.NORTH);
        add(inputPanel, BorderLayout.CENTER);
        add(southRegionPanel, BorderLayout.SOUTH);

        setVisible(true);
    }

    private JButton createOperatorButton(String text, Color bgColor) {
        JButton button = new JButton(text);
        button.setFont(new Font("SansSerif", Font.BOLD, 22));
        button.setForeground(BUTTON_FOREGROUND);
        button.setBackground(bgColor);
        button.setOpaque(true);
        button.setBorderPainted(false);
        button.setPreferredSize(new Dimension(60, 40));
        button.addActionListener(this);
        return button;
    }

    private ImageIcon loadImageIcon(String path) {
        URL imgURL = getClass().getClassLoader().getResource(path);
        if (imgURL != null) {
            return new ImageIcon(imgURL);
        } else {
            System.err.println("Resource not found: " + path);
            return null;
        }
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        try {
            double num1 = Double.parseDouble(num1Field.getText());
            double num2 = Double.parseDouble(num2Field.getText());
            double result = 0;
            String operation = "";

            if (e.getSource() == addButton) {
                operation = "\u2795";
                result = num1 + num2;
            } else if (e.getSource() == subtractButton) {
                operation = "-";
                result = num1 - num2;
            } else if (e.getSource() == multiplyButton) {
                operation = "\u00D7";
                result = num1 * num2;
            } else if (e.getSource() == divideButton) {
                operation = "/";
                if (num2 == 0) {
                    resultLabel.setText("Error: Division by zero");
                    return;
                }
                result = num1 / num2;
            }

            resultLabel.setText(" ");
            showResultDialog(result, operation, num1, num2);

        } catch (NumberFormatException ex) {
            resultLabel.setText("Error: Invalid input. Please enter numbers.");
        } catch (Exception ex) {
            resultLabel.setText("Error: Calculation failed.");
            ex.printStackTrace();
        }
    }

    private void showResultDialog(double result, String operation, double num1, double num2) {
        JDialog resultDialog = new JDialog(this, "Calculation Result", true);
        resultDialog.setSize(280, 150);
        resultDialog.setLocationRelativeTo(this);

        JPanel dialogPanel = new JPanel(new BorderLayout(10, 10));
        dialogPanel.setBorder(BorderFactory.createEmptyBorder(15, 15, 15, 15));

        String resultText = String.format("%.2f %s %.2f = %.4f", num1, operation, num2, result);
        JLabel dialogLabel = new JLabel(resultText, SwingConstants.CENTER);
        dialogLabel.setFont(new Font("SansSerif", Font.BOLD, 16));
        dialogLabel.setForeground(new Color(0, 100, 0));

        dialogPanel.add(dialogLabel, BorderLayout.CENTER);

        JButton closeButton = new JButton("OK");
        closeButton.addActionListener(e -> resultDialog.dispose());

        JPanel buttonPanel = new JPanel(new FlowLayout(FlowLayout.CENTER));
        buttonPanel.add(closeButton);
        dialogPanel.add(buttonPanel, BorderLayout.SOUTH);

        resultDialog.setContentPane(dialogPanel);
        resultDialog.setVisible(true);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(SimpleCalculator::new);
    }
}
```

---

## 📚 사용된 개념 상세 설명

| 개념 | 설명 |
|------|------|
| **JFrame** | 최상위 컨테이너 (윈도우) |
| **JPanel** | 컴포넌트들을 그룹화하는 컨테이너 |
| **BorderLayout / GridLayout / FlowLayout** | 레이아웃 매니저 |
| **ActionListener** | 버튼 클릭 이벤트 처리 |
| **JDialog** | 모달 결과 창 |
| **ImageIcon + ClassLoader** | 리소스 이미지 로딩 |
| **SwingUtilities.invokeLater** | EDT (Event Dispatch Thread)에서 GUI 생성 |
| **Unicode 문자** | `\u2795`, `\u00D7` 등 이모지 스타일 버튼 |

---

## 🔗 참고 자료

- [Java Swing Official Tutorial](https://docs.oracle.com/javase/tutorial/uiswing/) - Oracle
- [Java Swing 레이아웃 매니저 가이드](https://docs.oracle.com/javase/tutorial/uiswing/layout/index.html)
- [Java GUI Best Practices](https://www.oracle.com/java/technologies/best-practices.html)
- [Unicode Emoji & Symbols](https://unicode.org/emoji/charts/full-emoji-list.html)
- [IntelliJ IDEA로 Java Swing 프로젝트 만들기](https://www.jetbrains.com/idea/guide/)
- [Maven으로 Java Swing 프로젝트 설정하기](https://maven.apache.org/)

---

**Made with ❤️ using Java Swing**

필요하면 Maven 버전, Gradle 버전, 또는 다크모드 버전도 만들어 드릴 수 있습니다!
``` 

이 README를 프로젝트 루트에 `README.md`로 저장하면 GitHub 등에서 예쁘게 렌더링됩니다.