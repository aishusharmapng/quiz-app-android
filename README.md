# quiz-app-androidpackage com.example.quizapp;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    TextView question;
    RadioGroup radioGroup;
    RadioButton op1, op2, op3, op4;
    Button nextBtn;

    String[] questions = {
            "What is Android?",
            "Which language is used for Android?",
            "What is GitHub?"
    };

    String[][] options = {
            {"OS", "Browser", "Game", "Website"},
            {"Java", "HTML", "Python", "CSS"},
            {"Code editor", "Version control", "Browser", "OS"}
    };

    int[] answers = {0, 0, 1};
    int index = 0, score = 0;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        question = findViewById(R.id.tvQuestion);
        radioGroup = findViewById(R.id.radioGroup);
        op1 = findViewById(R.id.option1);
        op2 = findViewById(R.id.option2);
        op3 = findViewById(R.id.option3);
        op4 = findViewById(R.id.option4);
        nextBtn = findViewById(R.id.btnNext);

        loadQuestion();

        nextBtn.setOnClickListener(v -> {
            int selectedId = radioGroup.getCheckedRadioButtonId();
            if (selectedId == -1) {
                Toast.makeText(this, "Select an answer", Toast.LENGTH_SHORT).show();
                return;
            }

            RadioButton selected = findViewById(selectedId);
            int selectedIndex = radioGroup.indexOfChild(selected);

            if (selectedIndex == answers[index]) score++;

            index++;
            if (index < questions.length) {
                loadQuestion();
            } else {
                Intent i = new Intent(this, ResultActivity.class);
                i.putExtra("score", score);
                i.putExtra("total", questions.length);
                startActivity(i);
                finish();
            }
        });
    }

    void loadQuestion() {
        question.setText(questions[index]);
        op1.setText(options[index][0]);
        op2.setText(options[index][1]);
        op3.setText(options[index][2]);
        op4.setText(options[index][3]);
        radioGroup.clearCheck();
    }
}
package com.example.quizapp;

import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class ResultActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_result);

        TextView result = findViewById(R.id.tvResult);

        int score = getIntent().getIntExtra("score", 0);
        int total = getIntent().getIntExtra("total", 0);

        result.setText("Your Score: " + score + "/" + total);
    }
}
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="20dp">

    <TextView
        android:id="@+id/tvQuestion"
        android:text="Question"
        android:textSize="18sp"
        android:textStyle="bold"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

    <RadioGroup
        android:id="@+id/radioGroup"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <RadioButton android:id="@+id/option1"/>
        <RadioButton android:id="@+id/option2"/>
        <RadioButton android:id="@+id/option3"/>
        <RadioButton android:id="@+id/option4"/>
    </RadioGroup>

    <Button
        android:id="@+id/btnNext"
        android:text="Next"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
</LinearLayout>
