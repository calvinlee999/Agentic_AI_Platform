# Amazon Q Developer: Transforming Java with Claude Sonnet 4.5

## Assessment-First Java Modernization with AI

---

## Overview

**Amazon Q Developer** is AWS's AI-powered coding assistant that transforms Java applications using an **assessment-first approach**. Powered by **Claude Sonnet 4.5** through Amazon Bedrock, Amazon Q Developer analyzes your entire Java codebase upfront, identifies breaking changes, and creates a detailed modernization plan before making any code changes.

**Key Capabilities:**

| Feature | Benefit |
|---------|---------|
| **Pre-Transformation Assessment** | Understand impact before changing code |
| **Java Version Upgrades** | Automated Java 8 → 11/17/21 migration |
| **SQL Dialect Conversion** | Embedded Oracle → PostgreSQL transformation |
| **Dependency Management** | Automatic pom.xml/build.gradle updates |
| **IDE Integration** | Native VS Code and IntelliJ IDEA support |
| **Breaking Change Detection** | Identify issues before transformation |
| **Claude Sonnet 4.5 Powered** | 200K context window for deep analysis |
| **Natural Language Chat** | Ask questions about findings and recommendations |

---

## Why Amazon Q Developer for Java?

### The Challenge: Traditional Java Modernization

**Manual Approach Problems:**

❌ **Time-Consuming**
- Weeks of manual code review to understand current state
- Days of refactoring for each Java version upgrade
- Months for SQL dialect conversion

❌ **Error-Prone**
- Miss breaking changes in deprecated APIs
- Overlook dependency conflicts
- Introduce bugs during manual refactoring

❌ **Expensive**
- Senior developers spending weeks on repetitive tasks
- Extended testing cycles for manual changes
- Risk of production incidents from missed issues

❌ **Limited Visibility**
- No upfront assessment of transformation complexity
- Unknown scope until deep into the work
- Difficulty estimating timeline and cost

### The Solution: Assessment-First with Amazon Q Developer

**AI-Powered Approach Advantages:**

✅ **Fast Assessment**
- Minutes to analyze 100K+ lines of Java code
- Immediate identification of breaking changes
- Instant dependency compatibility check

✅ **Predictable Outcomes**
- Detailed pre-transformation report
- Know exactly what will change before starting
- Accurate effort estimation

✅ **High Automation**
- 80%+ of code changes automated
- Consistent refactoring patterns
- Reduced manual review time

✅ **Claude Sonnet 4.5 Intelligence**
- Deep semantic understanding of business logic
- Smart refactoring that preserves functionality
- Learns your coding patterns and standards

---

## How It Works: The 4-Phase Workflow

### Phase 1: Repository Integration (1 Minute)

Connect Amazon Q Developer to your Java project through:

**VS Code Extension:**
```bash
# Install extension
code --install-extension amazonwebservices.amazon-q-vscode

# Open Java project
cd /path/to/java-project
code .
```

**IntelliJ IDEA Plugin:**
```bash
# Install via JetBrains Marketplace
# Search: "AWS Toolkit"
```

**AWS CLI:**
```bash
# Configure AWS credentials
aws configure

# Install Amazon Q CLI
pip install amazon-q-developer-cli
```

**Supported Source Control:**
- GitHub
- GitLab
- Bitbucket
- AWS CodeCommit

---

### Phase 2: Pre-Transformation Assessment (2-10 Minutes)

Amazon Q analyzes your codebase and generates a comprehensive report.

**What Gets Analyzed:**

1. **Current Java Version**
   - Detect Java 8, 11, 17, or 21
   - Identify JDK-specific features in use

2. **Dependencies**
   - Parse Maven pom.xml or Gradle build.gradle
   - Check third-party library versions
   - Identify transitive dependencies

3. **Breaking Changes**
   - Deprecated APIs removed in target version
   - Changed API signatures
   - Removed JDK modules (JAXB, JAX-WS, CORBA)

4. **Code Patterns**
   - Date/Time API usage (java.util.Date vs java.time)
   - Reflection usage requiring updates
   - SecurityManager deprecations
   - Unsafe API calls

5. **Transformation Complexity**
   - Estimate % auto-fixable
   - Identify manual review requirements
   - Calculate effort estimation

**Example Assessment Interaction:**

```text
Developer: "Amazon Q, assess my Java project for Java 17 upgrade"

Amazon Q: "Analyzing your project... Please wait."
          [2 minutes later]
          
          "✅ Assessment Complete!
          
          📊 Project Overview:
          - Current Version: Java 8 (1.8.0_291)
          - Target Version: Java 17
          - Build Tool: Maven 3.6.3
          - Lines of Code: 45,000
          - Total Dependencies: 23 (18 direct, 5 transitive)
          - Total Files: 312 Java files
          
          ⚠️ Breaking Changes Detected (4 categories):
          
          1. JAXB Removed from JDK (HIGH severity)
             - Affected: 12 files (DataMapper.java, XmlSerializer.java, ...)
             - Occurrences: 47
             - Fix: Add jakarta.xml.bind-api:3.0.1 dependency
             - Auto-fixable: YES
          
          2. Date/Time API (MEDIUM severity)
             - Affected: 8 files (DateUtils.java, ReportGenerator.java, ...)
             - Occurrences: 134
             - Fix: Migrate to java.time package
             - Auto-fixable: YES
          
          3. SecurityManager Deprecated (MEDIUM severity)
             - Affected: 3 files (AppSecurityManager.java, ...)
             - Occurrences: 8
             - Fix: Use OS-level security or remove
             - Auto-fixable: NO (requires manual review)
          
          4. Unsafe Reflection (LOW severity)
             - Affected: 2 files (ReflectionHelper.java, ...)
             - Occurrences: 5
             - Fix: Use MethodHandles or VarHandles
             - Auto-fixable: NO (requires manual review)
          
          📦 Dependency Updates Required (18):
          
          ✅ Compatible (automatic update):
          - spring-framework: 4.3.25 → 5.3.23
          - hibernate-core: 5.2.17 → 5.6.14
          - jackson-databind: 2.8.11 → 2.13.4
          [... 15 more]
          
          ⚠️ Needs Manual Review:
          - custom-internal-library: 1.2.0 (no Java 17 version available)
          
          📈 Transformation Summary:
          - Auto-fixable: 85% (38,250 lines)
          - Manual review: 15% (6,750 lines)
          - Estimated automated time: 2 hours
          - Estimated manual review: 3 days
          - Testing effort: 1 week
          
          💡 Recommendations:
          1. Update dependencies before code transformation
          2. Test JAXB changes thoroughly (47 occurrences)
          3. Review SecurityManager usage carefully (security-critical)
          4. Update CI/CD pipeline to Java 17
          5. Plan extended testing for Date/Time API changes
          
          🎯 Strangler Pattern Candidates:
          - Module: legacy-xml-processing (Heavy JAXB usage)
            Reason: 47 JAXB occurrences, good isolation boundary
            Recommendation: Create new module with modern XML processing,
                            gradually route traffic to new implementation
          
          Would you like to proceed with transformation?"
```

---

### Phase 3: Transformation Planning (Interactive)

Review assessment findings with Amazon Q's AI agent through natural language chat.

**Example Conversations:**

**Understanding Risks:**
```text
Developer: "What's the risk with the SecurityManager deprecation?"

Amazon Q: "The SecurityManager deprecation in your AppSecurityManager.java 
          carries MEDIUM risk:
          
          1. Current Usage:
             - SecurityManager.checkPermission() for file access control
             - 8 occurrences across security layer
             - Used in: FileUploadHandler, ConfigLoader, DataExporter
          
          2. Why It's Deprecated:
             - Rarely used correctly in practice
             - OS-level security is more effective
             - Performance overhead
             - Complex permission models
          
          3. Recommended Approaches:
             
             Option A: Remove (preferred for most cases)
             - Rely on OS file permissions
             - Use Spring Security for authorization
             - Implement at application layer
             
             Option B: Continue with Java 17 (temporary)
             - SecurityManager still works in Java 17
             - Deprecated for removal in future version
             - Buy time for proper replacement
             
             Option C: Modern Alternatives
             - Use Files.setPosixFilePermissions() for file access
             - Implement custom authorization layer
             - Use cloud IAM roles (AWS IAM, Azure RBAC)
          
          4. Migration Steps:
             - Audit all 8 checkPermission() calls
             - Determine if security checks still needed
             - Replace with appropriate modern pattern
             - Update security tests
          
          Would you like me to show code examples for each approach?"

Developer: "Yes, show me Option A with Spring Security"

Amazon Q: "Here's how to replace SecurityManager with Spring Security:

[Shows before/after code with Spring Security method-level authorization,
 @PreAuthorize annotations, and custom SecurityEvaluator]

This approach gives you:
✅ More granular control
✅ Better integration with Spring ecosystem
✅ Easier testing
✅ No deprecated APIs

Would you like me to generate this replacement for all 8 occurrences?"
```

**Exploring Dependency Updates:**
```text
Developer: "Why do I need to update Spring Framework to 5.3.23?"

Amazon Q: "Spring Framework 4.3.25 → 5.3.23 update is required for Java 17:
          
          1. Java 17 Compatibility:
             - Spring 4.x officially supports Java 8-11 only
             - Spring 5.3.x adds Java 17 support
             - Includes Java 17 runtime optimizations
          
          2. Breaking Changes in Spring 5.x:
             
             ⚠️ Package Restructuring:
             - org.springframework.web.bind.annotation imports unchanged
             - org.springframework.mock.web moved (affects tests)
             
             ⚠️ Removed Deprecated Methods:
             - JdbcTemplate.queryForObject(String, Class) removed
               → Use JdbcTemplate.queryForObject(String, Class, Object...)
             - HttpStatus.series() method signature changed
          
          3. What Amazon Q Will Update:
             ✅ pom.xml dependency versions
             ✅ Import statements (if needed)
             ✅ Method calls to non-deprecated alternatives
             ✅ Test mock object packages
          
          4. Benefits of Upgrading:
             - Performance improvements (10-15% faster)
             - Better reactive programming support
             - Enhanced Kotlin support
             - Security fixes for Spring vulnerabilities
          
          Your code uses Spring extensively (89 files). I'll handle
          all updates automatically. Review changes in PR after transformation."
```

---

### Phase 4: Automated Transformation (Minutes to Hours)

Amazon Q executes the transformation plan, creating a new branch with all changes.

**Transformation Process:**

```text
Developer: "Proceed with Java 17 upgrade"

Amazon Q: "Starting transformation... Creating branch: amazonq-java17-upgrade
          
          ⏳ Phase 1: Backup and Branch Creation (30 seconds)
             ✅ Created branch: amazonq-java17-upgrade
             ✅ Switched to new branch
          
          ⏳ Phase 2: Dependency Updates (2 minutes)
             ✅ Updated pom.xml Java version 8 → 17
             ✅ Updated spring-framework 4.3.25 → 5.3.23
             ✅ Updated hibernate-core 5.2.17 → 5.6.14
             ✅ Updated jackson-databind 2.8.11 → 2.13.4
             ✅ Added jakarta.xml.bind-api 3.0.1 (JAXB replacement)
             ✅ Added jaxb-runtime 3.0.2
             [... 12 more dependencies]
             ✅ Resolved 3 dependency conflicts
          
          ⏳ Phase 3: Code Transformation (15 minutes)
             ✅ Transformed 156 files
             ✅ Fixed 47 JAXB references
                - Updated imports: javax.xml.bind → jakarta.xml.bind
                - Updated JAXBContext initialization
             ✅ Migrated 134 Date/Time API calls
                - java.util.Date → java.time.LocalDate (78)
                - java.util.Calendar → java.time.LocalDateTime (42)
                - SimpleDateFormat → DateTimeFormatter (14)
             ✅ Updated 23 deprecated API usages
                - Thread.stop() → interrupt() + flag
                - Applet removed (not used)
             ✅ Applied 89 modern Java patterns
                - try-with-resources for AutoCloseable
                - Stream API for collections
                - var keyword for local variables
             ✅ Generated migration notes
          
          ⚠️ Phase 4: Manual Review Items Flagged (pending)
             ⚠️ 8 SecurityManager calls → TRANSFORMATION_NOTES.md
             ⚠️ 5 reflection patterns → TRANSFORMATION_NOTES.md
             ⚠️ 1 dependency without Java 17 version → REVIEW_REQUIRED.md
          
          ⏳ Phase 5: Build Verification (3 minutes)
             ✅ Running: mvn clean compile
             ✅ Compilation: SUCCESSFUL (0 errors, 0 warnings)
             ✅ Running: mvn checkstyle:check
             ✅ Code style: PASSED
             ✅ Running: mvn test -DskipTests=false
             ⚠️ Tests: 234 passed, 3 failed (see details below)
          
          ⚠️ Test Failures:
             1. DateUtilsTest.testLegacyDateConversion
                - Expected: java.util.Date
                - Actual: java.time.LocalDate
                - Fix: Update test expectations
             
             2. XmlSerializerTest.testNamespaceHandling
                - JAXB namespace changed: javax → jakarta
                - Fix: Update test XML fixtures
             
             3. SecurityManagerTest.testFilePermissions
                - SecurityManager calls flagged for review
                - Fix: After SecurityManager replacement
          
          📝 Transformation Complete! Summary:
          
          ✅ Successful Changes:
             - 156 files modified
             - 2,847 lines changed
             - 18 dependencies updated
             - 2 new dependencies added
             - 234/237 tests passing (98.7%)
          
          ⚠️ Requires Your Review:
             - 3 test failures to fix
             - 8 SecurityManager calls (see TRANSFORMATION_NOTES.md)
             - 5 reflection patterns (see TRANSFORMATION_NOTES.md)
             - 1 dependency compatibility issue (see REVIEW_REQUIRED.md)
          
          📊 Files Created:
             - TRANSFORMATION_NOTES.md: Detailed changes and rationale
             - REVIEW_REQUIRED.md: Manual review items with recommendations
             - MIGRATION_GUIDE.md: Deployment instructions
             - ROLLBACK_PLAN.md: How to rollback if needed
          
          🔗 Next Steps:
             1. Review PR: https://github.com/yourorg/banking-app/pull/456
             2. Fix 3 test failures (guidance in PR comments)
             3. Review manual items in TRANSFORMATION_NOTES.md
             4. Run full test suite: mvn clean test
             5. Deploy to staging environment
             6. Merge PR when validated
          
          💬 Ask me anything about the transformation!
          Examples:
          - "Explain why DateUtilsTest failed"
          - "Show me the JAXB changes in DataMapper.java"
          - "What's the migration path for SecurityManager?"
          "

Developer: "Explain why DateUtilsTest failed"

Amazon Q: "The DateUtilsTest failure is expected and easy to fix:
          
          Problem:
          - Old code returned java.util.Date
          - New code returns java.time.LocalDate (modern API)
          - Test expected Date, got LocalDate
          
          Fix Required in DateUtilsTest.java:
          
          [Shows diff with updated test expectations using LocalDate,
           proper assertions, and test data updates]
          
          This is a safe change because:
          ✅ Business logic unchanged
          ✅ More precise (no time component confusion)
          ✅ Thread-safe (immutable)
          ✅ Better timezone handling
          
          Would you like me to create a commit with this test fix?"
```

---

### Phase 5: Validation & Testing

Amazon Q helps you validate the transformation and merge changes.

**Validation Checklist:**

1. **Review Generated Documentation**
   - Read TRANSFORMATION_NOTES.md for all changes
   - Understand rationale for each refactoring
   - Review REVIEW_REQUIRED.md for manual items

2. **Fix Test Failures**
   - Update test expectations for API changes
   - Fix test fixtures for namespace changes
   - Add new tests for refactored code

3. **Manual Review Items**
   - Address SecurityManager replacements
   - Review custom reflection patterns
   - Resolve dependency compatibility issues

4. **Run Full Test Suite**
   ```bash
   mvn clean test
   # Target: 100% passing
   ```

5. **Build and Package**
   ```bash
   mvn clean package
   # Verify JAR/WAR builds successfully
   ```

6. **Deploy to Staging**
   ```bash
   # Deploy transformed code to staging environment
   # Run smoke tests
   # Monitor for issues
   ```

7. **Performance Testing**
   - Compare performance with Java 8 version
   - Java 17 often 10-15% faster
   - Monitor memory usage

8. **Merge and Deploy**
   - Create PR from amazonq-java17-upgrade branch
   - Get code review from team
   - Merge to main after approval
   - Deploy to production with monitoring

---

## Key Features

### 1. Java Version Upgrades

**Supported Upgrade Paths:**

| From | To | Typical Scenarios |
|------|-----|-------------------|
| Java 8 | Java 11 | Security updates, LTS support |
| Java 8 | Java 17 | Modern features, performance gains |
| Java 8 | Java 21 | Latest LTS, cutting-edge features |
| Java 11 | Java 17 | Next LTS upgrade |
| Java 11 | Java 21 | Latest LTS upgrade |
| Java 17 | Java 21 | Stay current |

**What Gets Transformed:**

✅ **Language Features**
- Deprecated API replacements
- Modern syntax adoption (var, switch expressions, records)
- Improved pattern matching

✅ **Date/Time Handling**
- java.util.Date → java.time.LocalDate
- Calendar → LocalDateTime
- SimpleDateFormat → DateTimeFormatter

✅ **Concurrency Updates**
- synchronized → ReentrantLock
- Thread primitives → CompletableFuture
- Executor framework enhancements

✅ **JDK Module Changes**
- Add dependencies for removed modules (JAXB, JAX-WS, CORBA)
- Update javax.* → jakarta.* namespaces

**Example Transformation:**

**Before (Java 8):**
```java
import java.util.Date;
import java.util.Calendar;
import javax.xml.bind.*;

public class OrderService {
    // Old date handling
    public Date calculateDeadline(Date orderDate) {
        Calendar cal = Calendar.getInstance();
        cal.setTime(orderDate);
        cal.add(Calendar.DAY_OF_MONTH, 30);
        return cal.getTime();
    }
    
    // JAXB (removed in Java 11+)
    public String toXml(Order order) throws JAXBException {
        JAXBContext context = JAXBContext.newInstance(Order.class);
        Marshaller marshaller = context.createMarshaller();
        StringWriter writer = new StringWriter();
        marshaller.marshal(order, writer);
        return writer.toString();
    }
}
```

**After (Java 17):**
```java
import java.time.LocalDate;
import jakarta.xml.bind.*;
import java.io.StringWriter;

public class OrderService {
    // Modern java.time API
    public LocalDate calculateDeadline(LocalDate orderDate) {
        return orderDate.plusDays(30);
    }
    
    // Updated JAXB with Jakarta namespace
    public String toXml(Order order) throws JAXBException {
        JAXBContext context = JAXBContext.newInstance(Order.class);
        Marshaller marshaller = context.createMarshaller();
        marshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
        
        var writer = new StringWriter();
        marshaller.marshal(order, writer);
        return writer.toString();
    }
}
```

**Dependencies Updated (pom.xml):**
```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
</properties>

<dependencies>
    <!-- Jakarta XML Binding (JAXB was removed from JDK) -->
    <dependency>
        <groupId>jakarta.xml.bind</groupId>
        <artifactId>jakarta.xml.bind-api</artifactId>
        <version>3.0.1</version>
    </dependency>
    <dependency>
        <groupId>org.glassfish.jaxb</groupId>
        <artifactId>jaxb-runtime</artifactId>
        <version>3.0.2</version>
    </dependency>
</dependencies>
```

---

### 2. Embedded SQL Conversion (Oracle → PostgreSQL)

Amazon Q Developer transforms Oracle-specific SQL embedded in Java code to PostgreSQL-compatible syntax.

**What Gets Converted:**

✅ **SQL Syntax Differences**
- Oracle DECODE → PostgreSQL CASE
- Oracle NVL → PostgreSQL COALESCE
- Oracle SYSDATE → PostgreSQL CURRENT_TIMESTAMP
- Oracle (+) outer joins → PostgreSQL LEFT/RIGHT JOIN
- Oracle ROWNUM → PostgreSQL LIMIT/OFFSET
- Oracle sequences → PostgreSQL SERIAL/IDENTITY

✅ **JDBC Configuration**
- Replace Oracle JDBC driver with PostgreSQL
- Update connection strings
- Adjust connection pool settings

**Example SQL Transformation:**

**Before (Oracle):**
```java
public class UserRepository {
    private JdbcTemplate jdbcTemplate;
    
    public List<User> getRecentActiveUsers(int maxResults) {
        String sql = """
            SELECT user_id, username, email, created_date,
                   DECODE(status, 'ACTIVE', 'Active User', 
                                   'INACTIVE', 'Inactive User',
                                   'Unknown') as status_label
            FROM users
            WHERE status = 'ACTIVE'
              AND created_date >= SYSDATE - 30
              AND ROWNUM <= ?
            ORDER BY created_date DESC
            """;
        
        return jdbcTemplate.query(sql, this::mapUser, maxResults);
    }
    
    public String getUserDisplayStatus(Long userId, String defaultStatus) {
        String sql = "SELECT NVL(status_display, ?) FROM users WHERE user_id = ?";
        return jdbcTemplate.queryForObject(sql, String.class, defaultStatus, userId);
    }
    
    private User mapUser(ResultSet rs, int rowNum) throws SQLException {
        return new User(
            rs.getLong("user_id"),
            rs.getString("username"),
            rs.getString("email"),
            rs.getDate("created_date")
        );
    }
}
```

**After (PostgreSQL):**
```java
public class UserRepository {
    private JdbcTemplate jdbcTemplate;
    
    public List<User> getRecentActiveUsers(int maxResults) {
        String sql = """
            SELECT user_id, username, email, created_date,
                   CASE status 
                       WHEN 'ACTIVE' THEN 'Active User'
                       WHEN 'INACTIVE' THEN 'Inactive User'
                       ELSE 'Unknown'
                   END as status_label
            FROM users
            WHERE status = 'ACTIVE'
              AND created_date >= CURRENT_TIMESTAMP - INTERVAL '30 days'
            ORDER BY created_date DESC
            LIMIT ?
            """;
        
        return jdbcTemplate.query(sql, this::mapUser, maxResults);
    }
    
    public String getUserDisplayStatus(Long userId, String defaultStatus) {
        String sql = "SELECT COALESCE(status_display, ?) FROM users WHERE user_id = ?";
        return jdbcTemplate.queryForObject(sql, String.class, defaultStatus, userId);
    }
    
    private User mapUser(ResultSet rs, int rowNum) throws SQLException {
        return new User(
            rs.getLong("user_id"),
            rs.getString("username"),
            rs.getString("email"),
            rs.getDate("created_date")
        );
    }
}
```

**Database Configuration Updated (application.properties):**
```properties
# Before (Oracle)
spring.datasource.url=jdbc:oracle:thin:@localhost:1521:ORCL
spring.datasource.driver-class-name=oracle.jdbc.OracleDriver
spring.datasource.username=myuser
spring.datasource.password=mypass

# After (PostgreSQL)
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.username=myuser
spring.datasource.password=mypass
```

**Dependency Updated (pom.xml):**
```xml
<!-- Before -->
<dependency>
    <groupId>com.oracle.database.jdbc</groupId>
    <artifactId>ojdbc8</artifactId>
    <version>21.1.0.0</version>
</dependency>

<!-- After -->
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.5.1</version>
</dependency>
```

---

### 3. IDE Integration

Amazon Q Developer provides seamless integration with your development environment.

**VS Code Features:**
- ✅ Inline code transformation preview
- ✅ Diff view for before/after comparison
- ✅ Chat panel for Q&A about findings
- ✅ One-click acceptance of changes
- ✅ Automatic branch creation
- ✅ PR generation with detailed description

**IntelliJ IDEA Features:**
- ✅ Native IDE integration
- ✅ Refactoring preview window
- ✅ Built-in diff viewer
- ✅ Chat assistant in tool window
- ✅ Quick fixes for manual review items
- ✅ Maven/Gradle sync after dependency updates

---

### 4. Natural Language Interaction

Ask Amazon Q Developer questions about your assessment and transformation.

**Example Queries:**

```text
"Why was this file changed?"
"Show me all JAXB-related changes"
"What's the risk level of this transformation?"
"How do I test these date/time changes?"
"Explain the Spring Framework update"
"What breaking changes affect my code?"
"Generate unit tests for the refactored code"
"Create a deployment checklist"
```

---

## Integration with Amazon Bedrock and Claude Sonnet 4.5

Amazon Q Developer leverages **Amazon Bedrock** and **Claude Sonnet 4.5** for intelligent analysis and transformation.

**Why Claude Sonnet 4.5 for Java?**

| Capability | How It Helps Java Transformation |
|-----------|----------------------------------|
| **200K Token Context** | Analyze entire Java project (~50K LOC) at once |
| **Advanced Reasoning** | Understand complex business logic and design patterns |
| **Code Generation** | Generate idiomatic Java 17 code with modern features |
| **Multi-File Understanding** | Track dependencies across packages and modules |
| **Tool Use** | Invoke build tools, run tests, analyze bytecode |
| **Agentic Workflows** | Plan → Assess → Transform → Validate autonomously |

**How Amazon Q Uses Claude Internally:**

```typescript
// Conceptual workflow (Amazon Q internal implementation)
import { BedrockRuntimeClient, InvokeModelCommand } from "@aws-sdk/client-bedrock-runtime";

class AmazonQJavaTransformer {
  private bedrock: BedrockRuntimeClient;
  
  constructor() {
    this.bedrock = new BedrockRuntimeClient({ region: "us-east-1" });
  }
  
  // Step 1: Use Claude for pre-transformation assessment
  async assessJavaProject(projectPath: string): Promise<AssessmentReport> {
    const javaFiles = await this.readJavaFiles(projectPath);
    const buildConfig = await this.parseBuildFile(projectPath);
    
    const prompt = `You are an expert Java architect. Analyze this codebase for Java 17 upgrade.

Project: ${buildConfig.name}
Current Java Version: ${buildConfig.javaVersion}
Target: Java 17
Files: ${javaFiles.length}
Dependencies: ${buildConfig.dependencies.length}

Analyze each file and identify:
1. Breaking changes for Java 17
2. Deprecated API usage
3. Dependency compatibility issues
4. Required refactoring patterns
5. Transformation complexity estimate

Java Source Code:
${javaFiles.map(f => `// ${f.path}\n${f.content}`).join('\n\n')}

Provide detailed assessment in JSON format.`;

    const response = await this.bedrock.send(new InvokeModelCommand({
      modelId: "anthropic.claude-sonnet-4-5-v2:0",
      contentType: "application/json",
      accept: "application/json",
      body: JSON.stringify({
        anthropic_version: "bedrock-2023-05-31",
        max_tokens: 100000,
        messages: [{ role: "user", content: prompt }],
        temperature: 0.2 // Low temperature for consistent analysis
      })
    }));

    const result = JSON.parse(new TextDecoder().decode(response.body));
    return JSON.parse(result.content[0].text);
  }
  
  // Step 2: Use Claude for code transformation
  async transformJavaCode(
    sourceCode: string,
    transformRules: TransformRule[]
  ): Promise<string> {
    const prompt = `Transform this Java 8 code to Java 17.

Transformation Rules:
${transformRules.map(r => `- ${r.name}: ${r.description}`).join('\n')}

Original Code:
\`\`\`java
${sourceCode}
\`\`\`

Requirements:
1. Apply all transformation rules
2. Preserve business logic exactly
3. Use modern Java 17 features appropriately
4. Add comments explaining major changes
5. Maintain code style and formatting
6. Generate idiomatic, production-ready code

Return only the transformed Java code with no additional commentary.`;

    const response = await this.bedrock.send(new InvokeModelCommand({
      modelId: "anthropic.claude-sonnet-4-5-v2:0",
      contentType: "application/json",
      accept: "application/json",
      body: JSON.stringify({
        anthropic_version: "bedrock-2023-05-31",
        max_tokens: 50000,
        messages: [{ role: "user", content: prompt }],
        temperature: 0.1 // Very low for deterministic code generation
      })
    }));

    const result = JSON.parse(new TextDecoder().decode(response.body));
    return result.content[0].text;
  }
  
  // Step 3: Use Claude for validation
  async validateTransformation(
    originalCode: string,
    transformedCode: string
  ): Promise<ValidationResult> {
    const prompt = `Validate this Java code transformation.

Original (Java 8):
\`\`\`java
${originalCode}
\`\`\`

Transformed (Java 17):
\`\`\`java
${transformedCode}
\`\`\`

Verify:
1. Business logic preserved exactly
2. All deprecated APIs replaced correctly
3. Error handling maintained
4. Performance not degraded
5. No new bugs introduced

Provide detailed validation report in JSON format.`;

    const response = await this.bedrock.send(new InvokeModelCommand({
      modelId: "anthropic.claude-sonnet-4-5-v2:0",
      contentType: "application/json",
      accept: "application/json",
      body: JSON.stringify({
        anthropic_version: "bedrock-2023-05-31",
        max_tokens: 20000,
        messages: [{ role: "user", content: prompt }],
        temperature: 0.2
      })
    }));

    const result = JSON.parse(new TextDecoder().decode(response.body));
    return JSON.parse(result.content[0].text);
  }
}
```

**Claude Opus 4 for Complex Enterprise Java:**

For large-scale Java applications with complex architectures, Amazon Q can leverage **Claude Opus 4** with **extended thinking**:

```typescript
// Use Claude Opus 4 for complex multi-module migration strategy
async function planEnterpriseJavaMigration(
  project: EnterpriseJavaProject
): Promise<MigrationStrategy> {
  const bedrock = new BedrockRuntimeClient({ region: "us-east-1" });
  
  const prompt = `You are architecting a complex Java migration for an enterprise application.

<thinking>
Analyze this multi-module Maven project comprehensively:

Architecture:
- 50+ Maven modules with complex dependencies
- Spring Framework 4.x → 6.x (requires Java 17)
- Java 8 → Java 17 upgrade
- Oracle database → PostgreSQL migration
- Embedded Tomcat → Cloud-native (AWS ECS)
- Monolithic → Microservices transition

Key Considerations:
1. Module dependency graph and build order
2. Breaking changes across all architectural layers
3. Database schema migration with zero downtime
4. Test coverage gaps and risk areas
5. Deployment strategy (blue/green, canary)
6. Rollback procedures and contingency plans
7. Team skill gaps and training needs
8. Timeline and resource allocation

Constraints:
- Zero downtime requirement
- Must maintain backward compatibility
- Regulatory compliance (SOX, PCI-DSS)
- Budget: $500K, Timeline: 12 months
</thinking>

Project Details:
${JSON.stringify(project, null, 2)}

Create a comprehensive, phased migration strategy with:
- Detailed module-by-module transformation order
- Critical path analysis with dependencies
- Risk mitigation strategies for each phase
- Testing strategy (unit, integration, E2E, performance)
- Rollback procedures for each phase
- Team structure and skill requirements
- 12-month timeline with milestones
- Budget breakdown by phase
- Success metrics and KPIs

Provide strategy in structured JSON format.`;

  const response = await bedrock.send(new InvokeModelCommand({
    modelId: "anthropic.claude-opus-4-20250514:0",
    contentType: "application/json",
    accept: "application/json",
    body: JSON.stringify({
      anthropic_version: "bedrock-2023-05-31",
      max_tokens: 200000,
      messages: [{ role: "user", content: prompt }],
      temperature: 0.3,
      // Enable extended thinking for deep analysis
      thinking: {
        type: "enabled",
        budget_tokens: 50000
      }
    })
  }));

  const result = JSON.parse(new TextDecoder().decode(response.body));
  return JSON.parse(result.content[0].text);
}
```

---

## Best Practices

### Assessment Phase Best Practices

**DO:**

✅ **Run Assessment First**
- Always start with pre-transformation assessment
- Never skip straight to transformation
- Understand full scope before committing

✅ **Review Assessment Thoroughly**
- Read every breaking change
- Understand why each change is needed
- Ask Amazon Q questions about unclear items

✅ **Check Dependencies First**
- Verify all third-party libraries have compatible versions
- Resolve dependency conflicts before code transformation
- Test dependency updates in isolation

✅ **Use Chat for Clarification**
- Ask Amazon Q about risk levels
- Request examples for complex changes
- Get recommendations for manual review items

✅ **Document Current State**
- Capture current Java version and dependencies
- Note any custom configurations or workarounds
- Record business-critical code sections

---

### Transformation Phase Best Practices

**DO:**

✅ **Transform on a Branch**
- Let Amazon Q create a dedicated branch
- Never transform on main/master directly
- Enables easy rollback if needed

✅ **Review Generated Documentation**
- Read TRANSFORMATION_NOTES.md completely
- Understand rationale for each change
- Keep documentation for future reference

✅ **Fix Test Failures Immediately**
- Address broken tests before moving forward
- Update test expectations for API changes
- Add new tests for refactored code

✅ **Address Manual Review Items**
- Don't skip items flagged for manual review
- Carefully implement SecurityManager replacements
- Review custom reflection patterns

✅ **Test Incrementally**
- Run tests after each transformation phase
- Validate builds before proceeding
- Catch issues early

**DON'T:**

❌ **Skip Assessment**
- Never transform without understanding impact
- Blind transformations lead to production issues

❌ **Ignore Warnings**
- Manual review items exist for good reasons
- SecurityManager changes are security-critical
- Reflection patterns need careful review

❌ **Deploy Without Testing**
- Always test in staging first
- Run full regression test suite
- Perform performance testing

❌ **Forget CI/CD Updates**
- Update pipeline Java version
- Verify Docker base images use Java 17
- Check deployment scripts for Java version references

---

### Integration with Strangler Pattern

Amazon Q Developer's assessment helps identify **strangler pattern candidates** for gradual modernization.

**Example: Using Assessment to Plan Strangler Pattern:**

```typescript
import { AmazonQDeveloperClient, StartAssessmentCommand } from "@aws-sdk/client-amazonq-developer";

async function identifyStranglerCandidatesWithAmazonQ(
  projectPath: string
): Promise<StranglerCandidate[]> {
  const amazonQ = new AmazonQDeveloperClient({ region: "us-east-1" });
  
  // Step 1: Run assessment
  const assessment = await amazonQ.send(new StartAssessmentCommand({
    projectPath,
    assessmentType: "JAVA_VERSION_UPGRADE",
    targetVersion: "17"
  }));
  
  // Step 2: Wait for assessment completion
  let assessmentStatus = "IN_PROGRESS";
  while (assessmentStatus === "IN_PROGRESS") {
    await sleep(5000);
    const status = await amazonQ.send(new GetAssessmentStatusCommand({
      assessmentId: assessment.assessmentId
    }));
    assessmentStatus = status.status;
  }
  
  // Step 3: Get assessment results
  const results = await amazonQ.send(new GetAssessmentResultsCommand({
    assessmentId: assessment.assessmentId
  }));
  
  // Step 4: Identify strangler candidates from assessment
  const stranglerCandidates: StranglerCandidate[] = [];
  
  for (const breakingChange of results.breakingChanges) {
    // High-impact breaking changes are good strangler candidates
    if (breakingChange.severity === "HIGH" && 
        breakingChange.occurrences > 20 &&
        breakingChange.affectedFiles.length > 5) {
      
      // Example: Heavy JAXB usage suggests isolated XML processing module
      if (breakingChange.type === "REMOVED_API" && 
          breakingChange.description.includes("JAXB")) {
        
        stranglerCandidates.push({
          moduleName: "xml-processing-service",
          reason: `Heavy JAXB usage (${breakingChange.occurrences} occurrences across ${breakingChange.affectedFiles.length} files)`,
          affectedFiles: breakingChange.affectedFiles,
          recommendation: `Create new microservice for XML processing:
            1. Build modern XML processing service with Java 17 + Jakarta EE
            2. Deploy alongside legacy monolith
            3. Route new XML operations to new service
            4. Gradually migrate existing XML operations
            5. Decommission legacy XML code when migration complete`,
          complexity: "MEDIUM",
          estimatedEffort: "6 weeks",
          benefits: [
            "Isolate JAXB migration complexity",
            "Test new service independently",
            "Zero downtime migration",
            "Easy rollback if issues"
          ]
        });
      }
    }
  }
  
  return stranglerCandidates;
}

function sleep(ms: number): Promise<void> {
  return new Promise(resolve => setTimeout(resolve, ms));
}
```

**Strangler Pattern Workflow with Amazon Q:**

```text
1. Assessment Phase (Amazon Q)
   ↓
   Identify high-impact breaking changes
   ↓
   Flag modules with heavy deprecated API usage
   ↓
   Recommend strangler pattern for isolated modules

2. Strangler Implementation
   ↓
   Create new module/service with Java 17
   ↓
   Use Amazon Q to transform isolated component
   ↓
   Deploy new service alongside legacy

3. Traffic Routing
   ↓
   Route 10% traffic to new service
   ↓
   Monitor metrics and errors
   ↓
   Gradually increase to 100%

4. Legacy Decommissioning
   ↓
   Remove legacy code after validation
   ↓
   Complete transformation for that component
```

---

## Pricing and Quotas

### Free Tier

**Included:**
- $0/month
- 2,000 lines of code per month
- 100 transformation jobs per month
- 1,000 lines per job
- Basic IDE integration
- Assessment reports
- Community support

**Good For:**
- Small projects and prototypes
- Learning and experimentation
- Side projects and open source

---

### Pro Tier

**Price: $19/user/month**

**Included:**
- Unlimited lines of code transformations
- 1,000 transformation jobs per month
- No per-job line limit
- Advanced chat with Amazon Q
- Priority support
- Team collaboration features
- Enterprise SSO (with AWS IAM)

**Quotas:**
- Concurrent jobs: 1 per user, 100 per AWS account
- Jobs per month: 1,000

**Good For:**
- Professional development teams
- Enterprise Java modernization
- Production applications
- Teams with multiple projects

---

### Cost Estimation

```typescript
function estimateAmazonQCost(params: {
  linesOfCode: number;
  numberOfTransformations: number;
  teamSize: number;
}): CostEstimate {
  const { linesOfCode, numberOfTransformations, teamSize } = params;
  
  const FREE_TIER_LINES_PER_MONTH = 2000;
  const FREE_TIER_JOBS_PER_MONTH = 100;
  const PRO_TIER_MONTHLY_COST = 19;
  
  // Calculate if free tier is sufficient
  const totalLinesPerMonth = linesOfCode * numberOfTransformations;
  const fitsInFreeTier = 
    totalLinesPerMonth <= FREE_TIER_LINES_PER_MONTH &&
    numberOfTransformations <= FREE_TIER_JOBS_PER_MONTH;
  
  if (fitsInFreeTier) {
    return {
      tier: "FREE",
      monthlyCost: 0,
      annualCost: 0,
      recommendation: "Free tier is sufficient for your usage"
    };
  }
  
  // Calculate Pro tier cost
  const proMonthlyCost = PRO_TIER_MONTHLY_COST * teamSize;
  const proAnnualCost = proMonthlyCost * 12;
  
  // Calculate manual transformation cost for comparison
  const manualCostPerThousandLines = 40; // Senior dev hourly rate
  const manualTotalCost = (linesOfCode / 1000) * manualCostPerThousandLines * numberOfTransformations;
  
  const savings = manualTotalCost - proMonthlyCost;
  const roi = ((savings / proMonthlyCost) * 100).toFixed(0);
  
  return {
    tier: "PRO",
    monthlyCost: proMonthlyCost,
    annualCost: proAnnualCost,
    manualCost: manualTotalCost,
    savings: savings,
    roi: `${roi}%`,
    recommendation: `Pro tier saves $${savings.toLocaleString()} (${roi}% ROI)`
  };
}

// Example: 50K LOC Java 8 → 17 project, 5-person team
const estimate = estimateAmazonQCost({
  linesOfCode: 50000,
  numberOfTransformations: 1,
  teamSize: 5
});

console.log(estimate);
// Output:
// {
//   tier: "PRO",
//   monthlyCost: 95,
//   annualCost: 1140,
//   manualCost: 2000,
//   savings: 1905,
//   roi: "2005%",
//   recommendation: "Pro tier saves $1,905 (2005% ROI)"
// }
```

---

### ROI by Project Size

| Project Size | Manual Cost | Amazon Q Pro (5 users) | Savings | ROI |
|-------------|-------------|------------------------|---------|-----|
| **10K LOC** | $400 (1 week) | $95/month | $305 | 321% |
| **50K LOC** | $2,000 (1 month) | $95/month | $1,905 | 2,005% |
| **100K LOC** | $4,000 (2 months) | $95/month | $3,905 | 4,111% |
| **500K LOC** | $20,000 (6 months) | $95/month | $19,905 | 20,953% |
| **1M LOC** | $40,000 (1 year) | $95/month | $39,905 | 42,005% |

*Assumptions:*
- Manual transformation: $40/1K LOC (senior developer time)
- Amazon Q Pro: $19/user/month for 5-person team
- Does not include testing time (similar for both approaches)

---

## Getting Started

### Prerequisites

✅ **AWS Account**
- Active AWS account with billing enabled
- IAM permissions for Amazon Q Developer

✅ **Java Project**
- Maven or Gradle project
- Java 8, 11, 17, or 21
- Source code in Git repository

✅ **IDE**
- VS Code with Amazon Q extension, OR
- IntelliJ IDEA with AWS Toolkit

✅ **AWS CLI** (optional for command-line usage)
- AWS CLI v2 installed
- Configured with credentials

---

### Quick Start (5 Minutes)

**Step 1: Install Amazon Q Extension**

```bash
# For VS Code
code --install-extension amazonwebservices.amazon-q-vscode

# For IntelliJ IDEA
# Install via JetBrains Marketplace: Search "AWS Toolkit"
```

**Step 2: Configure AWS Credentials**

```bash
aws configure
# AWS Access Key ID: [your-key]
# AWS Secret Access Key: [your-secret]
# Default region: us-east-1
# Default output format: json
```

**Step 3: Open Java Project**

```bash
cd /path/to/your-java-project
code .  # Opens in VS Code
```

**Step 4: Start Assessment**

1. Open Command Palette: `Cmd+Shift+P` (Mac) or `Ctrl+Shift+P` (Windows/Linux)
2. Type: "Amazon Q: Transform"
3. Select: "Java Version Upgrade" or "SQL Conversion"
4. Choose target version (e.g., Java 17)
5. Review assessment report

**Step 5: Execute Transformation**

1. After reviewing assessment, click "Proceed"
2. Amazon Q creates new branch with changes
3. Review PR and merge when validated

---

### Full Implementation Timeline

| Week | Phase | Activities | Deliverables |
|------|-------|------------|-------------|
| **Week 1** | Assessment & Planning | - Install Amazon Q Developer<br>- Run pre-transformation assessment<br>- Review breaking changes<br>- Identify strangler candidates<br>- Create transformation strategy | - Assessment report<br>- Breaking change inventory<br>- Transformation plan<br>- Resource allocation |
| **Week 2** | Pilot Transformation | - Select 1-2 small modules<br>- Run Amazon Q transformation<br>- Fix manual review items<br>- Validate builds and tests<br>- Document lessons learned | - Transformed pilot modules<br>- Build validation<br>- Test results<br>- Lessons learned doc |
| **Week 3-4** | Bulk Transformation | - Transform remaining modules<br>- Address test failures<br>- Fix manual review items<br>- Update CI/CD pipeline<br>- Performance testing | - All modules transformed<br>- 100% tests passing<br>- Updated CI/CD<br>- Performance benchmarks |
| **Week 5** | Testing & Validation | - Full regression testing<br>- Integration testing<br>- Performance testing<br>- Security review<br>- Documentation updates | - Test results<br>- Performance report<br>- Security audit<br>- Updated docs |
| **Week 6** | Staging Deployment | - Deploy to staging environment<br>- User acceptance testing<br>- Monitor for issues<br>- Fix any bugs found | - Staging deployment<br>- UAT results<br>- Bug fixes<br>- Monitoring dashboards |
| **Week 7** | Production Rollout | - Deploy to production (gradual)<br>- Monitor metrics closely<br>- Be ready for rollback<br>- Collect feedback | - Production deployment<br>- Metrics monitoring<br>- Incident response plan<br>- Feedback collection |
| **Week 8** | Retrospective & Closure | - Team retrospective<br>- Update runbooks<br>- Knowledge transfer<br>- Celebrate success! | - Retrospective notes<br>- Updated runbooks<br>- Knowledge base<br>- Success metrics |

---

### Resources

**Official Documentation:**
- [Amazon Q Developer User Guide](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/)
- [Transforming Java Applications](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/transform-java.html)
- [Amazon Bedrock - Claude](https://aws.amazon.com/bedrock/anthropic/)

**Training & Tutorials:**
- [AWS Training: Amazon Q Developer](https://aws.amazon.com/training/learn-about/developer-tools/)
- [Java Migration Best Practices](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-java/)
- [Strangler Fig Pattern](https://martinfowler.com/bliki/StranglerFigApplication.html)

**Community & Support:**
- [AWS re:Post - Amazon Q](https://repost.aws/tags/TALmcXzmfeRaKOzrBowJ9cJQ)
- [AWS Developer Forums](https://forums.aws.amazon.com/)
- [GitHub - AWS Samples](https://github.com/aws-samples)

**Blog Posts & Case Studies:**
- [Introducing Amazon Q Developer](https://aws.amazon.com/blogs/aws/amazon-q-developer/)
- [Java Modernization with Amazon Q](https://aws.amazon.com/blogs/devops/)
- [Claude in Amazon Bedrock](https://aws.amazon.com/blogs/machine-learning/)

---

## Summary

Amazon Q Developer provides an **assessment-first approach** to Java modernization, powered by Claude Sonnet 4.5 through Amazon Bedrock. Key benefits:

✅ **Fast Assessment**: Minutes instead of weeks to understand impact
✅ **Predictable Outcomes**: Know exactly what will change before starting
✅ **High Automation**: 80%+ of code changes automated
✅ **Claude Intelligence**: Deep semantic understanding of business logic
✅ **IDE Integration**: Seamless workflow in VS Code and IntelliJ IDEA
✅ **Natural Language**: Chat with AI about findings and recommendations
✅ **Cost Effective**: $19/user/month with significant ROI

**Perfect For:**
- Java 8 → 17/21 upgrades
- Oracle → PostgreSQL migrations
- Spring Framework modernization
- Enterprise Java assessment
- Gradual strangler pattern adoption

**Next Steps:**
1. Install Amazon Q Developer extension
2. Run assessment on your Java project
3. Review findings and ask questions
4. Execute transformation with confidence

---

*For more information, see [Lens 0: Assessment & Foundation Phase](README.md)*

*Last Updated: November 17, 2025*
