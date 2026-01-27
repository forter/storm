# storm

> This file provides context for AI coding assistants working with this repository.

## Repository Overview

Apache Storm is a free and open-source distributed real-time computation system. It's designed to reliably process unbounded streams of data, doing for real-time processing what Hadoop did for batch processing.

**Key Features**:
- Scalable and fault-tolerant real-time data processing
- Supports multiple programming languages (Java, Clojure, Python)
- Integrates with various queueing and database technologies

**Tech Stack**: Java (primary), Clojure, Python, JavaScript, HTML

**Project Type**: Distributed System Framework

**Key Relationships**:
- Works with ZooKeeper for cluster coordination
- Integrates with various messaging systems (Kafka, RabbitMQ, etc.)
- Can be used with Hadoop YARN for resource management

## Build System & Available Commands

Apache Storm uses Maven as its build system.

**Key Commands**:
```
mvn clean install             # Build Storm and run all tests
mvn clean install -DskipTests # Build without running tests
mvn eclipse:eclipse           # Generate Eclipse project files
mvn package                   # Create JAR files
```

**Environment Setup**:
- Ensure JAVA_HOME is set to a JDK 8+ installation
- Set STORM_SRC_ROOT_DIR to the root of the Storm source code

## Essential Workflows

### Initial Setup (Step-by-Step)

1. Prerequisites:
   - JDK 8 or later
   - Maven 3.x
   - Python (for some scripts)

2. Clone the repository:
   ```
   git clone https://github.com/apache/storm.git
   cd storm
   ```

3. Build Storm:
   ```
   mvn clean install -DskipTests
   ```

4. Set up environment variables:
   ```
   export STORM_SRC_ROOT_DIR=/path/to/storm
   export JAVA_HOME=/path/to/jdk
   ```

5. Verify installation:
   ```
   bin/storm version
   ```

### Local Development

1. Run Storm in local mode:
   ```
   bin/storm local
   ```

2. Submit a topology for testing:
   ```
   bin/storm jar /path/to/your-topology-jar.jar org.apache.storm.MyTopology
   ```

3. Use the Storm UI for monitoring (typically at http://localhost:8080)

### Making Code Changes

1. Make changes in your preferred IDE
2. Rebuild the project:
   ```
   mvn clean install
   ```
3. If changing Java API, update the multi-lang adapters in `storm-multilang`
4. Run relevant tests:
   ```
   mvn test -pl storm-core,storm-server,storm-client
   ```

### Running Tests

- Run all tests:
  ```
  mvn test
  ```
- Run tests for a specific module:
  ```
  mvn test -pl storm-core
  ```
- Run a specific test class:
  ```
  mvn test -pl storm-core -Dtest=TestBolt
  ```

Test files are organized under `test/` directories in each module.

## Project Structure

```
📁 bin/              # Storm executables and scripts
📁 conf/             # Configuration files and examples
📁 dev-tools/        # Development utilities
📁 docs/             # Documentation and website source
📁 examples/         # Example Storm topologies
📁 external/         # External dependencies and integrations
📁 storm-core/       # Core Storm functionality
📁 storm-client/     # Client-side Storm libraries
📁 storm-server/     # Server-side Storm components
📁 storm-webapp/     # Storm UI web application
📁 storm-multilang/  # Multi-language support (Python, JS, etc.)
```

## Domain-Specific Patterns

Storm uses a specific terminology and concepts:

- **Topology**: The overall computation, analogous to a MapReduce job
- **Spout**: Source of streams in a topology
- **Bolt**: Processes input streams and produces output streams
- **Tuple**: The basic unit of data in Storm
- **Stream**: An unbounded sequence of tuples

Example Topology in Java:

```java
TopologyBuilder builder = new TopologyBuilder();
builder.setSpout("word-spout", new TestWordSpout());
builder.setBolt("word-count", new TestWordCounter())
       .fieldsGrouping("word-spout", new Fields("word"));
```

## Critical Patterns & Conventions

- Use `TopologyBuilder` to construct topologies
- Implement `IRichBolt` or `IRichSpout` for custom components
- Use `Fields` to define tuple structures
- Leverage `Config` class for topology configuration
- Follow the "at-least-once" processing guarantee pattern

## Common Pitfalls & Gotchas

- Ensure proper serialization for custom types in tuples
- Be cautious with long-running bolts, they can block the worker
- Remember to acknowledge tuples to ensure proper fault tolerance
- Avoid overusing global variables in spouts and bolts
- Ensure proper cleanup in the `cleanup()` method of bolts and spouts

## Testing Philosophy

- Unit tests for individual components (spouts, bolts)
- Integration tests for full topologies
- Use `LocalCluster` for testing topologies in local mode
- Leverage `MockedSources` and `MockedSink` for controlled testing

## CI/CD Pipeline

- Uses Jenkins for Continuous Integration
- Automated tests run on pull requests
- Release process involves Apache Release Manager

## Security Guidelines (CRITICAL - Forter Standards)

**Must Follow**:
- ❌ NEVER hardcode secrets, API keys, or credentials
- ❌ NEVER log sensitive data (PII, tokens, passwords)
- ❌ NEVER commit .env files or secrets
- ✅ Use environment variables for all secrets
- ✅ Validate and sanitize all user inputs
- ✅ Follow the principle of least privilege

Additional Storm-specific guidelines:
- Use Storm's security features (JAAS, Kerberos) when deploying
- Secure ZooKeeper connections
- Implement proper ACLs for Nimbus and Supervisor daemons

## Quick Reference Commands

| Task                  | Command                                    |
|-----------------------|--------------------------------------------|
| Build Storm           | `mvn clean install -DskipTests`            |
| Run all tests         | `mvn test`                                 |
| Generate Eclipse files| `mvn eclipse:eclipse`                      |
| Run Storm locally     | `bin/storm local`                          |
| Submit topology       | `bin/storm jar <jar-file> <topology-class>`|

## Before Committing Checklist

1. Run unit tests: `mvn test`
2. Verify code style: `mvn checkstyle:check`
3. Ensure no new compiler warnings
4. Update documentation if necessary
5. Run integration tests if applicable
6. Verify changes in local mode before submitting

---
*Generated by Forter AI Platform*