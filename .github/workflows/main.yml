        builder.setNegativeButton("Û‹Ø§Ø² ÙƒÛ•Ú†ØªÙ‰Ù…", (dialog, which) -> dialog.cancel());
        builder.show();
    }
}


======================================================================
7. LibraryFragment.java (Â«ÙƒÛ‡ØªÛ‡Ù¾Ø®Ø§Ù†Ø§Â» Ø¨Û†Ù„Ù‰ÙƒÙ‰: Ø¦Ù‰Ú†ÙƒÙ‰ Ø³Ø§Ù‚Ù„Ù‰ØºÛ‡Ú†Ù†Ù‰ Ø³Ù‰ÙƒØ§Ù†Ù‰Ø±Ù„Ø§Ø´ Û‹Û• Ø¨ÛØ³Ù‰Ù¾ ØªÛ‡Ø±Û‡Ù¾ ÙƒÙ‰Ø±Ú¯ÛˆØ²ÛˆØ´)
======================================================================

package com.example.bookreader;

import android.app.AlertDialog;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.Toast;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;

public class LibraryFragment extends Fragment {

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_library, container, false);

        // ØªÛÙ„ÛÙÙˆÙ†Ø¯Ù‰ÙƒÙ‰ Ø¨Ø§Ø±Ù„Ù‰Ù‚ PDF, TXT, DOCX Ù„Ø§Ø± ÙƒÛ†Ø±ÛˆÙ†Ù‰Ø¯Û‡
        // Ø¨ÛØ³Ù‰Ù¾ ØªÛ‡Ø±ØºØ§Ù†Ø¯Ø§ Â«Ù…ÛÙ†Ù‰Ú­Â» Ø¨Û†Ù„Ù‰ÙƒÙ‰Ú¯Û• ÙƒÙ‰Ø±Ú¯ÛˆØ²ÛˆØ´ ÙƒÛ†Ø²Ù†Ù‰ÙƒÙ‰ Ø¦ÛÚ†Ù‰Ù„Ù‰Ø¯Û‡
        return view;
    }

    private void showAddToCollectionDialog(String fileName) {
        String[] collections = {"ØªØ§Ø±Ù‰Ø®Ù‰ÙŠ ÙƒÙ‰ØªØ§Ø¨Ù„Ø§Ø±", "Ù¾Û•Ù„Ø³Û•Ù¾Ù‰Û‹Ù‰ÙŠ ÙƒÙ‰ØªØ§Ø¨Ù„Ø§Ø±"};
        AlertDialog.Builder builder = new AlertDialog.Builder(getContext());
        builder.setTitle("ØªÙˆÙ¾Ù„Ø§Ù…ØºØ§ ÙƒÙ‰Ø±Ú¯ÛˆØ²ÛˆØ´");
        builder.setItems(collections, (dialog, which) -> {
            Toast.makeText(getContext(), fileName + " -> " + collections[which] + " ØºØ§ ÙƒÙ‰Ø±Ú¯ÛˆØ²ÛˆÙ„Ø¯Ù‰", Toast.LENGTH_SHORT).show();
        });
        builder.show();
    }
}


======================================================================
8. QuranZikirFragment.java (Â«Ù‚Û‡Ø±Ø¦Ø§Ù† Û‹Û• Ø²Ù‰ÙƒÙ‰Ø±Â» Ø¨Û†Ù„Ù‰ÙƒÙ‰)
======================================================================

package com.example.bookreader;

import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;

public class QuranZikirFragment extends Fragment {

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        // Ø¦ÛÙƒØ±Ø§Ù†Ø¯Ø§ Â«Ø¨Û‡ ÙŠÛ•Ø± Ú¾Ø§Ø²Ù‰Ø±Ú†Û• ØªÙˆÙ„Û‡Ù‚Ù„Ø§Ù†Ù…Ù‰Ø¯Ù‰. ÙƒÛØ±Û•ÙƒÙ„Ù‰Ùƒ Ø¦Ù‰Ù‚ØªÙ‰Ø¯Ø§Ø±Ù„Ø§Ø± ÙƒÛÙŠÙ‰Ù†ÙƒÙ‰ Ù†Û•Ø´Ø±Ú¯Û• Ù‚Ø§Ù„Ø¯Û‡Ø±Û‡Ù„Ø¯Ù‰Â» Ø¯Û•Ù¾ Ú†Ù‰Ù‚Ù‰Ø¯Û‡
        return inflater.inflate(R.layout.fragment_quran_zikir, container, false);
    }
}


======================================================================
9. ReaderActivity.java (PDF Ø¦ÙˆÙ‚Û‡ØºÛ‡Ú† Û‹Û• Ø¦Ø§Ù¾ØªÙˆÙ…Ø§ØªÙ‰Ùƒ Ø¨Û•Øª Ø³Ø§Ù‚Ù„Ø§Ø´)
======================================================================

package com.example.bookreader;

import android.content.SharedPreferences;
import android.net.Uri;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import com.github.barteksc.pdfviewer.PDFView;
import com.github.barteksc.pdfviewer.listener.OnPageChangeListener;

public class ReaderActivity extends AppCompatActivity implements OnPageChangeListener {

    private static final String PREFS_NAME = "BookProgressPrefs";
    private PDFView pdfView;
    private Uri pdfUri;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_reader);

        pdfView = findViewById(R.id.pdfView);
        
        if (getIntent().getData() != null) {
            pdfUri = getIntent().getData();
            int savedPage = getSavedPageProgress(pdfUri.toString());
            
            pdfView.fromUri(pdfUri)
                    .defaultPage(savedPage)
                    .enableSwipe(true)
                    .enableDoubletap(true)
                    .enableAntialiasing(true)
                    .onPageChange(this)
                    .load();
        }
    }

    @Override
    public void onPageChanged(int page, int pageCount) {
        if (pdfUri != null) {
            savePageProgress(pdfUri.toString(), page);
        }
    }

    private void savePageProgress(String uriKey, int page) {
        SharedPreferences prefs = getSharedPreferences(PREFS_NAME, MODE_PRIVATE);
        prefs.edit().putInt("page_" + uriKey.hashCode(), page).apply();
    }

    private int getSavedPageProgress(String uriKey) {
        SharedPreferences prefs = getSharedPreferences(PREFS_NAME, MODE_PRIVATE);
        return prefs.getInt("page_" + uriKey.hashCode(), 0);
    }
}
