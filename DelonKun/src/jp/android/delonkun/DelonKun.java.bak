package jp.android.delonkun;

import android.app.Activity;
import android.os.Bundle;

import java.util.ArrayList;

import android.app.Activity;
import android.content.ActivityNotFoundException;
import android.content.Intent;
import android.media.AudioManager;
import android.media.ToneGenerator;
import android.os.Bundle;
import android.speech.RecognizerIntent;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.TextView;
import android.widget.Toast;

public class DelonKun extends Activity {
	final int MODE_LOW = 0;
	final int MODE_MID = 1;
	final int MODE_HIGH = 2;

	final int TONE_RIGHT = ToneGenerator.TONE_DTMF_2;	//ホントは1
	final int TONE_LEFT = ToneGenerator.TONE_DTMF_1;	//ホントは2
	final int TONE_FORWARD = ToneGenerator.TONE_DTMF_3;
	final int TONE_STOP = ToneGenerator.TONE_DTMF_4;

	ToneGenerator toneGenerator;
	int g_mode = MODE_MID;

	private static final int REQUEST_CODE = 0;

	private void activateRecognize() {
		try {
			Intent intent = new Intent(RecognizerIntent.ACTION_RECOGNIZE_SPEECH);
			intent.putExtra(RecognizerIntent.EXTRA_LANGUAGE_MODEL,
					RecognizerIntent.LANGUAGE_MODEL_FREE_FORM);
			intent.putExtra(RecognizerIntent.EXTRA_PROMPT,
					"Delon-kun Voice Recognition");
			startActivityForResult(intent, REQUEST_CODE);

			int a = 1;

		} catch (ActivityNotFoundException e) {
			e.printStackTrace();
		}
	}

	public void pushTone(int i, int mode) {
		int t = 150;
		final TextView txtStatus = (TextView)findViewById(R.id.txtStatus);

		t += mode * 150;
		if (i == ToneGenerator.TONE_DTMF_3)
			t += 300;

		// Toast.makeText(this, String.valueOf(i), Toast.LENGTH_LONG).show();
		txtStatus.setText(String.format("mode %d dir %d time %d",mode, i, t));
/* 動作コマンド鳴動～停止 */
		toneGenerator.startTone(i);
		try {
			Thread.sleep(150);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		toneGenerator.stopTone();
/* 待つ */
		try {
			Thread.sleep(t);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
/* 停止コマンド鳴動～停止 */
		toneGenerator.startTone(ToneGenerator.TONE_DTMF_4);
		try {
			Thread.sleep(150);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		toneGenerator.stopTone();
/*
		toneGenerator.startTone(ToneGenerator.TONE_DTMF_4);
		try {
			Thread.sleep(100);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		toneGenerator.stopTone();
		try {
			Thread.sleep(100);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
*/
	}

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);

		final TextView txtStatus = (TextView)findViewById(R.id.txtStatus);
		toneGenerator = new ToneGenerator(AudioManager.STREAM_SYSTEM,
			ToneGenerator.MAX_VOLUME);

		pushTone(TONE_STOP, g_mode);

		Button btn1 = (Button) findViewById(R.id.btnRecognize);
		btn1.setOnClickListener(new OnClickListener() {
			public void onClick(View v) {
				activateRecognize();
			}
		});

		Button btnStop = (Button) findViewById(R.id.btnStop);
		btnStop.setOnClickListener(new OnClickListener() {
			public void onClick(View v) {
				pushTone(TONE_STOP, g_mode);
			}
		});

	}

	@Override
	protected void onActivityResult(int requestCode, int resultCode, Intent data) {
		// Toast.makeText(this, String.valueOf(resultCode),
		// Toast.LENGTH_LONG).show();
		if (requestCode == REQUEST_CODE && resultCode == RESULT_OK) {
			String resultsString = "";
			ArrayList<String> results = data
					.getStringArrayListExtra(RecognizerIntent.EXTRA_RESULTS);
			for (int i = 0; i < results.size(); i++) {
				resultsString += results.get(i);
			}
			int localmode = g_mode;

			Toast.makeText(this, resultsString, Toast.LENGTH_LONG).show();
			// MODE
			if (resultsString.contains("少し"))
				localmode = MODE_LOW;
			else if (resultsString.contains("たくさん"))
				localmode = MODE_HIGH;

			// DIRECTION
			// 右
			if (resultsString.contains("右"))
				pushTone(TONE_RIGHT, localmode);
			else if (resultsString.contains("みぎ"))
				pushTone(TONE_RIGHT, localmode);
			else if (resultsString.contains("ミニ"))
				pushTone(TONE_RIGHT, localmode);
			else if (resultsString.contains("虹"))
				pushTone(TONE_RIGHT, localmode);
			// 左
			if (resultsString.contains("左"))
				pushTone(TONE_LEFT, localmode);
			else if (resultsString.contains("ひだり"))
				pushTone(TONE_LEFT, localmode);
			// 前
			if (resultsString.contains("前"))
				pushTone(TONE_FORWARD, localmode);
			else if (resultsString.contains("前進"))
				pushTone(TONE_FORWARD, localmode);
			else if (resultsString.contains("全身"))
				pushTone(TONE_FORWARD, localmode);
			// 停止
			if (resultsString.contains("停止"))
				pushTone(TONE_STOP, localmode);
			else if (resultsString.contains("天使"))
				pushTone(TONE_STOP, localmode);

			activateRecognize();
		} else {

		}
		super.onActivityResult(requestCode, resultCode, data);
	}
}
