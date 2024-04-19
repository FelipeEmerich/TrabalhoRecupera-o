import java.sql.*;

public class Main {
    static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
    static final String DB_URL = "jdbc:mysql://localhost:3306/SEU_BANCO_DE_DADOS";
    static final String USER = "SEU_USUARIO";
    static final String PASS = "SUA_SENHA";

    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try {
            Class.forName(JDBC_DRIVER);

            System.out.println("Conectando ao banco de dados...");
            conn = DriverManager.getConnection(DB_URL, USER, PASS);

            stmt = conn.createStatement();

            String createTableSql = "CREATE TABLE IF NOT EXISTS PESSOA (" +
                                    "CPF VARCHAR(11) PRIMARY KEY," +
                                    "NOME VARCHAR(100)," +
                                    "DATA_NASCIMENTO DATE," +
                                    "CIDADANIA VARCHAR(50))";
            stmt.executeUpdate(createTableSql);

            String insertSql = "INSERT INTO PESSOA (CPF, NOME, DATA_NASCIMENTO, CIDADANIA) " +
                               "VALUES ('12345678900', 'Fulano', '1990-01-01', 'Brasileira')";
            stmt.executeUpdate(insertSql);
            System.out.println("Registro inserido com sucesso!");

            String updateSql = "UPDATE PESSOA SET NOME = 'Novo Nome' WHERE CPF = '12345678900'";
            stmt.executeUpdate(updateSql);
            System.out.println("Registro atualizado com sucesso!");

            String deleteSql = "DELETE FROM PESSOA WHERE CPF = '12345678900'";
            stmt.executeUpdate(deleteSql);
            System.out.println("Registro excluído com sucesso!");

            String selectSql = "SELECT * FROM PESSOA";
            ResultSet rs = stmt.executeQuery(selectSql);

            while (rs.next()) {
                String cpf = rs.getString("CPF");
                String nome = rs.getString("NOME");
                Date dataNascimento = rs.getDate("DATA_NASCIMENTO");
                String cidadania = rs.getString("CIDADANIA");

                System.out.println("CPF: " + cpf + ", Nome: " + nome + ", Data de Nascimento: " + dataNascimento + ", Cidadania: " + cidadania);
            }

            
            rs.close();
            stmt.close();
            conn.close();
        } catch (SQLException se) {
          
            se.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (stmt != null) stmt.close();
            } catch (SQLException se2) {
            }
            try {
                if (conn != null) conn.close();
            } catch (SQLException se) {
                se.printStackTrace();
            }
        }
        System.out.println("Fim da execução.");
    }
}
