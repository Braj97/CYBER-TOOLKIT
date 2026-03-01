# CYBER-TOOLKIT
#TCL
# ============================================
#         CYBER TOOLKIT IN TCL
#         Author: Braj Kishor Das
# ============================================

# ===== Encryption Procedure =====
proc encryptText {text key} {
    set result ""
    set keyLength [string length $key]
    
    for {set i 0} {$i < [string length $text]} {incr i} {
        set char [string index $text $i]
        set keyChar [string index $key [expr {$i % $keyLength}]]
        
        set ascii [scan $char %c]
        set keyAscii [scan $keyChar %c]
        
        set encryptedChar [format %c [expr {($ascii + $keyAscii) % 256}]]
        append result $encryptedChar
    }
    return $result
}

# ===== Decryption Procedure =====
proc decryptText {text key} {
    set result ""
    set keyLength [string length $key]
    
    for {set i 0} {$i < [string length $text]} {incr i} {
        set char [string index $text $i]
        set keyChar [string index $key [expr {$i % $keyLength}]]
        
        set ascii [scan $char %c]
        set keyAscii [scan $keyChar %c]
        
        set decryptedChar [format %c [expr {($ascii - $keyAscii + 256) % 256}]]
        append result $decryptedChar
    }
    return $result
}

# ===== Password Generator =====
proc generatePassword {length} {
    set chars "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#\$%^&*"
    set password ""
    
    for {set i 0} {$i < $length} {incr i} {
        set index [expr {int(rand() * [string length $chars])}]
        append password [string index $chars $index]
    }
    return $password
}

# ===== File Analyzer =====
proc analyzeFile {filename} {
    if {![file exists $filename]} {
        puts "File does not exist!"
        return
    }
    
    set file [open $filename r]
    set content [read $file]
    close $file

    set lines [llength [split $content "\n"]]
    set words [llength [split $content]]
    set chars [string length $content]

    puts "\n--- File Analysis ---"
    puts "Lines      : $lines"
    puts "Words      : $words"
    puts "Characters : $chars"
}

# ===== Pattern Scanner =====
proc patternScan {filename pattern} {
    if {![file exists $filename]} {
        puts "File does not exist!"
        return
    }

    set file [open $filename r]
    set lineNumber 0

    puts "\n--- Pattern Results ---"

    while {[gets $file line] >= 0} {
        incr lineNumber
        if {[string match "*$pattern*" $line]} {
            puts "Found at Line $lineNumber : $line"
        }
    }
    close $file
}

# ===== Main Menu =====
while {1} {
    puts "\n========== CYBER TOOLKIT =========="
    puts "1. Encrypt Text"
    puts "2. Decrypt Text"
    puts "3. Generate Password"
    puts "4. Analyze File"
    puts "5. Scan Pattern in File"
    puts "6. Exit"
    puts "Enter your choice: "

    gets stdin choice

    switch $choice {
        1 {
            puts "Enter Text:"
            gets stdin text
            puts "Enter Key:"
            gets stdin key
            set encrypted [encryptText $text $key]
            puts "Encrypted Text: $encrypted"
        }
        2 {
            puts "Enter Encrypted Text:"
            gets stdin text
            puts "Enter Key:"
            gets stdin key
            set decrypted [decryptText $text $key]
            puts "Decrypted Text: $decrypted"
        }
        3 {
            puts "Enter Password Length:"
            gets stdin len
            puts "Generated Password: [generatePassword $len]"
        }
        4 {
            puts "Enter File Name:"
            gets stdin fname
            analyzeFile $fname
        }
        5 {
            puts "Enter File Name:"
            gets stdin fname
            puts "Enter Pattern to Search:"
            gets stdin pattern
            patternScan $fname $pattern
        }
        6 {
            puts "Exiting..."
            break
        }
        default {
            puts "Invalid Choice!"
        }
    }
}
